---
title: react源码解析01
date: 2019-07-11 11:02:46
tags: react  源码
---

### react hook api 的动机
原因
1. 很难重用组件之间的state状态逻辑，会使用复杂的高阶组件导致很多的包裹层。
2. 复杂的组件会变得很难理解
3. 类的概念让开发很难理解，javascript中this 的概念跟大多数语言都不太一样。


### react核心概念

    
    fiber

    FiberRootNode
    FiberNode
    Effects

FiberRootNode
```
function FiberRootNode(containerInfo, tag, hydrate) {
  this.tag = tag;
  this.current = null;
  this.containerInfo = containerInfo;
  this.pendingChildren = null;
  this.pingCache = null;
  this.finishedExpirationTime = NoWork;
  this.finishedWork = null;
  this.timeoutHandle = noTimeout;
  this.context = null;
  this.pendingContext = null;
  this.hydrate = hydrate;
  this.firstBatch = null;
  this.callbackNode = null;
  this.callbackExpirationTime = NoWork;
  this.firstPendingTime = NoWork;
  this.lastPendingTime = NoWork;
  this.pingTime = NoWork;
 }
 
export function createFiberRoot(
  containerInfo: any,
  tag: RootTag,
  hydrate: boolean,
): FiberRoot {
  const root: FiberRoot = (new FiberRootNode(containerInfo, tag, hydrate): any);
  // Cyclic construction. This cheats the type system right now because
  // stateNode is any.
  const uninitializedFiber = createHostRootFiber(tag);
  root.current = uninitializedFiber;
  uninitializedFiber.stateNode = root;
  return root;
}

```
Fibernode
```
function FiberNode(
  tag: WorkTag,
  pendingProps: mixed,
  key: null | string,
  mode: TypeOfMode,
) {
  // Instance
  this.tag = tag;
  this.key = key;
  this.elementType = null;
  this.type = null;
  this.stateNode = null;

  // Fiber
  this.return = null;
  this.child = null;
  this.sibling = null;
  this.index = 0;

  this.ref = null;

  this.pendingProps = pendingProps;
  this.memoizedProps = null;
  this.updateQueue = null;
  this.memoizedState = null;
  this.dependencies = null;

  this.mode = mode;

  // Effects
  this.effectTag = NoEffect;
  this.nextEffect = null;

  this.firstEffect = null;
  this.lastEffect = null;

  this.expirationTime = NoWork;
  this.childExpirationTime = NoWork;

  this.alternate = null;
  
}
```
    
### react 源码关键函数

```
updateContainer


updateContainerAtExpirationTime


scheduleRootUpdate



scheduleWork

		renderRoot

			workLoop/workLoopSync

				performUnitOfWork
				
					beginWork
					
					completeUnitOfWork


		commitRoot

			commitRootImpl
			
				commitBeforeMutationEffects
				
					commitBeforeMutationEffectOnFiber
				
				commitMutationEffects

					commitResetTextContent

					commitDetachRef
				
				commitLayoutEffects

					
				


```