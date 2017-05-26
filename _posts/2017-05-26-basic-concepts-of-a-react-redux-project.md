---
layout: post
title: 一个基于react-redux项目的基本数据流流程
date: 2017-05-26
categories: blog
description: 前端学习
---

# 一个基于react-redux项目的基本数据流流程       
这两天通过公司的验证码系统熟悉了一下项目结构,项目是基于react,react-redux,redux-saga等的,下面是一些笔记.       
主要记录的是完成一个完整的小功能的基本流程.       

#### 加载完页面后,开始通过api请求数据,所以在这里dispatch一个action.       

```
// index.js
  componentDidMount() {
    console.log('componentDidMount: ', this.props);
    this.props.dispatch(info());
  }
```

#### 这里的dispatch方法是从redux里传上来的,通过这个mapDispatchToProps函数       

```
//index.js
function mapDispatchToProps(dispatch) {
  return {
    onChangeTest: () => dispatch(info()),
    dispatch,
  };
}
```
(可以把redux看为下面的数据层,也就是M,而上层就是react,是V)       

#### 第一步中dispatch的action是写在actions.js中的       

```
// actions.js
import {
  INFO,
  INFO_SUCCESS,
} from './constants';

export function info() {
  return {
    type: INFO,
  };
}

// constants.js
export const INFO = 'app/BasicInfo/INFO';
export const INFO_SUCCESS = 'app/BasicInfo/INFO_SUCCESS';
```

#### dispatch这个action之后,因为在saga中有监视这个action,所以会进入监视后调用的basicInfo函数       
**redux-saga一句话解释作用就是用它来集中处理所有异步请求.**       

```
// sagas.js
export function* basicInfoWatcher() {
  console.log('basicInfoWatcher!');
  yield takeLatest(INFO, basicInfo);
}
```

#### 以下是basicInfo函数,进行执行异步抓取数据,如果响应成功(res.response),就发起另外一个action:infoAccepted       

```
// sagas.js
export function* basicInfo() {
  console.log('basicInfo!');
  const requestURL = '/web-api/center/info';
  const requestOptions = {
    method: 'GET',
  };
  const rsp = yield call(httpFetch, requestURL, requestOptions);
  if (rsp.response) {
    yield put(infoAccepted(rsp.response));
  }
  console.log('rsp: ', rsp);
}
```

#### infoAccepted action       

```
// actions.js
export function infoAccepted(response) {
  return {
    type: INFO_SUCCESS,
    payload: response,
  };
}
```

#### 发起了infoAccepted action之后,因为type为INFO_SUCCESS,所以会被reducer抓到,执行case里面的东西       

```
// reducer.js

import { fromJS } from 'immutable';
import {
  INFO_SUCCESS,
} from './constants';

const initialState = fromJS({
  account: '',
  user_id: '',
  score: 0,
});

function basicInfoReducer(state = initialState, action) {
  switch (action.type) {
    case INFO_SUCCESS:
      console.log('INFO_SUCCESS: ', action.payload.data.data);
      return state
        .set('account', action.payload.data.data.account)
        .set('user_id', action.payload.data.data.user_id)
        .set('score', action.payload.data.data.score);
    default:
      return state;
  }
}
```
**其实每dispatch一个action,都会进入reducer函数中,只是要看是否是匹配的case,不是的话,相当于只是default路过.**
**上面代码中的fromJS(), state.set()等方法都是immutablejs中的方法,而state.set()方法就是将数据写进redux的store中.**       
至此就完成了在react中dispatch一个action从服务器异步请求数据并保存到下层的redux的store中,下面就是将store中的数据传到上层的react中.       

#### 可以理解为,每一次state有改动,也即store里的数据有改动时,就会重新render一次       

```
// index.js
  render() {
    console.log('render');
    return (
      <Wrapper>
        <DIV>
          <span style={{ marginRight: 18, width: 70, display: 'inline-block' }}>
            帐号名:
          </span>
          <span>
            {this.props.BasicInfo.account === '' ? '***********' : this.props.BasicInfo.account}
          </span>
        </DIV>
      </Wrapper>
    );
  }
}
```
**也就是说,重新执行之后,就会重新检测一次this.props.BasicInfo.account的值,如果不为'',就把它的值显示到页面上,至此就完成了从redux里的store到react中最后再渲染到真正的DOM中.**       
不过在这中间,还有一步要说一下       

#### 通过mapStateToProps方法将redux的store里拿到的state传到react的props中,然后就可以用this.props.BasicInfo拿数据了       

```
// index.js
const mapStateToProps = createStructuredSelector({
  BasicInfo: makeSelectBasicInfo(),
});

// selectors.js
import { createSelector } from 'reselect';

const selectBasicInfoDomain = () => (state) => state.get('basicInfo');

const makeSelectBasicInfo = () => createSelector(
  selectBasicInfoDomain(),
  (substate) => substate.toJS()
);

export default makeSelectBasicInfo;
export {
  selectBasicInfoDomain,
};
```

#### Reselect       
mapStateToProps也被叫做selector，在store发生变化的时候就会被调用，而不管是不是selector关心的数据发生改变它都会被调用，所以如果selector计算量非常大，每次更新都重新计算可能会带来性能问题。Reselect能帮你省去这些没必要的重新计算。       
Reselect 提供 createSelector 函数来创建可记忆的 selector。createSelector 接收一个 input-selectors 数组和一个转换函数作为参数。如果 state tree 的改变会引起 input-selector 值变化，那么 selector 会调用转换函数，传入 input-selectors 作为参数，并返回结果。如果 input-selectors 的值和前一次的一样，它将会直接返回前一次计算的数据，而不会再调用一次转换函数。这样就可以避免不必要的计算，为性能带来提升。       
