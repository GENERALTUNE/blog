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