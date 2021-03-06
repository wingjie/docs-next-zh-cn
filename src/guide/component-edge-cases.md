# 处理边界情况

> 该页面假设你已经阅读过了[组件基础](component-basics.md)。如果你对组件还不太了解，推荐你先阅读它。

:::tip 提示
这里记录的都是和处理边界情况有关的功能，即一些需要对 Vue 的规则做一些小调整的特殊情况。不过注意，这些功能都是有劣势或危险的场景的。我们会在每个案例中注明，所以当你使用每个功能的时候请稍加留意。
:::

## 控制更新

得益于其响应性系统，Vue 总是知道何时更新 (如果你使用正确的话)。但是，在某些边缘情况下，你可能希望强制更新，尽管事实上没有任何响应式数据发生更改。还有一些情况下，你可能希望防止不必要的更新。

### 强制更新

如果你发现自己需要在 Vue 中强制更新，那么在 99.99% 的情况下，你已经在某个地方犯了错误。例如，你可能依赖于 Vue 响应性系统未跟踪的状态，比如在组件创建之后添加了 `data` 属性。

但是，如果你已经排除了上述情况，并且发现自己处于这种非常罕见的情况下，必须手动强制更新，那么你可以使用 [`$forceUpdate`](../api/instance-methods.html#forceupdate)。

### 低级静态组件与 `v-once`

在 Vue 中渲染纯 HTML 元素的速度非常快，但有时你可能有一个包含**很多**静态内容的组件。在这些情况下，可以通过向根元素添加 `v-once` 指令来确保只对其求值一次，然后进行缓存，如下所示：

```js
app.component('terms-of-service', {
  template: `
    <div v-once>
      <h1>Terms of Service</h1>
      ... a lot of static content ...
    </div>
  `
})
```

:::tip
再次提醒，不要过度使用这种模式。虽然在需要渲染大量静态内容的极少数情况下使用这种模式会很方便，但除非你注意到先前的渲染速度很慢，否则就没有必要这样做——另外，过度使用这种模式可能会在以后引起很多混乱。例如，假设另一个开发人员不熟悉 `v-once` 或者没有在模板中发现它，他们可能会花上几个小时来弄清楚为什么模板没有正确更新。
:::
