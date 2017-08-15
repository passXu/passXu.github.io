1. 在fragment中获得context
   fragment中需要使用adapter的时候，难免需要用到context，而fragment本身并不是一个context，想要得到context的话，可以使用`getActivity`，会返回与fragment绑定的avtivity。

2. app包中的fragment使用和v4包中fragment的使用的区别：
   v4包：activity需要继承`FragmentActivity`，得到FragmentManager 需要调用`getSupportFragmentManager`
   app包：activity需要继承`activity`就可以了，得到FragmentManager，调用

3. 一个activity中多个fragment切换，参考HomePage中的`switchFragment`方法。
4. fragment之间的通信，可以使用getActivity()，这样多个fragment就可以获得同一个Activity了。