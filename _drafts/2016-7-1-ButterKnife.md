1. 绑定OnClick的时候，view不需要BindViw，也可以使用，
2. android studio 里面引入ButteerKnife的时候m，要注意添加依赖包，并且声明当前module的build.gradle `apply plugin: 'com.neenbedankt.android-apt'` 注意是添加一条声明，而不是改掉原来的声明，需要添加的依赖包是 在modul中添加` apt 'com.jakewharton:butterknife-compiler:8.1.0'`在project中添加`classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'` 
3. 在其他地方使用butterknife的时候，
`static class ViewHolder {`
 `   @BindView(R.id.title) TextView name;`
`    @BindView(R.id.job_title) TextView jobTitle;`
`    public ViewHolder(View view) {`
  `    ButterKnife.bind(this, view);`
 `      }`
 ` }` 亲测如果去掉static也是可以使用的

-----------------------------------
使用的onclick注解的方法不能为static和private。