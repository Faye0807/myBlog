# requestAnimationFrame

> 告诉浏览器——你希望执行一个动画，并且要求浏览器在`下次重绘`之`前`调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘`之前`执行

当 `requestAnimationFrame`运行在后台标签页或者隐藏的<iframe> 里时，requestAnimationFrame() 会被暂停调用以提升性能和电池寿命。
