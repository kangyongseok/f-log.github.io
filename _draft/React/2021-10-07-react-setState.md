## 리액트의 setState 동작 이해

리액트에서 setState 는 비동기로 동작합니다. 그리고 setState 가 동일한 state 를 여러번 업데이트하려고 호출했을때 성능상 이슈로 한꺼번에 업데이트를 처리합니다.
서로 다른 state 값이더라도 별로도 여러번 호출 되었을경우 한번의 setState 가 발생한것처럼 동작하여 한번만 렌더링 시킵니다.  
이론적으로는 알고있는데 이걸 리액트에서는 코드를 어떻게 작성해서 풀어 나가고 있는지 정리해 보려고 합니다.

```js
// react/packages/react/src/ReactBaseClasses.js
/**
 * Sets a subset of the state. Always use this to mutate
 * state. You should treat `this.state` as immutable.
 *
 * There is no guarantee that `this.state` will be immediately updated, so
 * accessing `this.state` after calling this method may return the old value.
 *
 * There is no guarantee that calls to `setState` will run synchronously,
 * as they may eventually be batched together.  You can provide an optional
 * callback that will be executed when the call to setState is actually
 * completed.
 *
 * When a function is provided to setState, it will be called at some point in
 * the future (not synchronously). It will be called with the up to date
 * component arguments (state, props, context). These values can be different
 * from this.* because your function may be called after receiveProps but before
 * shouldComponentUpdate, and this new state, props, and context will not yet be
 * assigned to this.
 *
 * @param {object|function} partialState Next partial state or function to
 *        produce next partial state to be merged with current state.
 * @param {?function} callback Called after state is updated.
 * @final
 * @protected
 */
Component.prototype.setState = function (partialState, callback) {
  if (
    typeof partialState !== 'object' &&
    typeof partialState !== 'function' &&
    partialState != null
  ) {
    throw new Error(
      'setState(...): takes an object of state variables to update or a ' +
        'function which returns an object of state variables.',
    )
  }

  this.updater.enqueueSetState(this, partialState, callback, 'setState')
}
```

react 는 페이스북에서 만든 오픈소스라이브러리로 깃헙에서 검색하면 어떻게 코드가 작성되어있는지 확인할 수 있습니다.  
위 코드는 setState 부분의 소스만 가져온 코드입니다.
