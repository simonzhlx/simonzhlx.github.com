---
layout: post
title: Sync Gulp Tasks
permalink: /Sync-Gulp-Tasks/
---

I have already known that **Gulp** runs tasks in parallel and there're several ways to ensure tasks run in order.
* The 1st one is use [the gulp-sequence plugin](https://www.npmjs.com/package/gulp-sequence). This plugin takes an array of tasks and perform them one by one.
* The 2nd one is leveraging the dependencies of the tasks: the 2nd parameter of the task could be an array of other tasks that are the prerequisite of current task which means that current task won't start
until those tasks in the array are finished.  

However there's one thing should be noticed that **those prerequisite tasks might not actually complete** as some plugins called inside those tasks may run in parallel.
And for avoiding this problem, check whether the plugins leveraged in tasks have a callback that is invoked after its execution if those do, put a parameter as the callback for task's completion and invoke this 
parameter in the plugin's 'done' callback like following snippet:

```javascript
gulp.task('initialize', (cb) => {

  iterateFiles(config.TASKDIR, (fn) => { require(fn); }, (err) => {console.log('all tasks have been done');cb();});

});
```

Plus, if gulp stream is being used in the task function, make sure return the statement as below:

```javascript
gulp.task('clean', function() {
    return gulp.src('dist/*').pipe(rm());
});
```
