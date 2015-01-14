I will start this post off by saying that I like GruntJS and I have nothing against those who continue to use it. Grunt improved my workflow dramatically by providing a way for me to automate various cumbersome tasks like minification, JSLint and unit tests to name a few. Even though GruntJS works great I've never been a fan of its configuration file syntax; also GruntJS files aren't exactly "pretty" to look at either.
 
### Enter GulpJS

GulpJS is the new streaming build system in town that I have now used for little over a month; and in my opinion will replace GruntJS as the de facto JavaScript task runner. 
 
### So what exactly is GulpJS? 
 
GulpJS is a streaming build system; meaning gulp makes use of pipes for streaming data that needs to be processed. Gulp's syntax in my opinion is extremely simple compared to that of a grunt file. Namely because gulp uses a code-over-configuration convention that makes for a simpler and more intuitive build process, making your gulp files more readable. 
 
Now I know what many of you are saying "oh great, here's yet another task runner..." I was there too; but that was until I read the code to see how it all worked. That is when the excitement and fun began.

### Getting Started

Before I show you what a gulp file looks like, it's important to mention that GulpJS only has 5 methods! The following are all you will need to get started writing your tasks:

	gulp.src(globs[, options]);
	gulp.dest(path);
	gulp.task(name[, deps], fn);
	gulp.watch(glob [, opts], tasks); // or gulp.watch(glob [, opts, cb]);
	gulp.run(tasks...);
	
Here's how my gulpfile looks:

**gulpfile.js**

	/*global require*/

	(function () {
      'use strict';
  
      var gulp = require('gulp'),
          jshint = require('gulp-jshint'),
          mocha = require('gulp-mocha'),
          istanbul = require('gulp-istanbul'),
          clean = require('gulp-clean'),
          exec = require('gulp-exec'),
          plato = require('gulp-plato'),
          mversion = require('gulp-mversion');

      gulp.task('lint', function () {
          gulp.src(['./lib/*', './test/*', './gulpfile.js'])
              .pipe(jshint('.jshintrc'))
              .pipe(jshint.reporter('default'));
      });
  
      gulp.task('istanbul', function (callback) {
          gulp.src('./lib/*.js')
              .pipe(istanbul())
              .on('end', callback);
      });
  
      gulp.task('test', function () {
          gulp.src('./test/*.js').pipe(mocha({
              reporter: 'spec',
              bail: true,
              ui: 'bdd'
          }));
      });
  
      gulp.task('test-coverage', function () {
          gulp.run('istanbul', function () {
              gulp.src('./test/*.js')
                  .pipe(mocha())
                  .pipe(istanbul.writeReports())
                  .pipe(exec('echo Coverage has been calculated. See coverage
                  				 directory for details.'));
          });
      });
  
      gulp.task('code-report', function () {
          gulp.src('./lib/*.js')
              .pipe(plato('./report'))
              .pipe(exec('echo The code report has been generated. 
              				 See report directory for details.'));
      });
  
      gulp.task('reports', function () {
          gulp.run('test-coverage', 'code-report');
      });
  
      gulp.task('clean-reports', function () {
          gulp.src(['./coverage', './report'], {
              read: false
          })
              .pipe(clean({
                  force: true
              }));
          gulp.src('').pipe(exec('echo Coverage ^& Report 
          							 directories has been removed'));
      });
  
      gulp.task('dev', function () {
          gulp.run('lint', 'test');
          gulp.watch(['./lib/*', './test/*'], function () {
              gulp.run('lint', 'test');
          });
      });
  
      gulp.task('prod', function () {
          if (gulp.env.major) {
              gulp.run('lint', 'test');
              gulp.src('./package.json')
                  .pipe(mversion('major'))
                  .pipe(gulp.dest('./'));
          } else if (gulp.env.minor) {
              gulp.run('lint', 'test');
              gulp.src('./package.json')
                  .pipe(mversion('minor'))
                  .pipe(gulp.dest('./'));
          } else if (gulp.env.patch) {
              gulp.run('lint', 'test');
              gulp.src('./package.json')
                  .pipe(mversion('patch'))
                  .pipe(gulp.dest('./'));
          }
      });
	}());
    
### Gulpfile Breakdown

**GulpJS + Plugins**

      // Gulp
      var gulp = require('gulp'),
      
      	// Plugins
          jshint = require('gulp-jshint'),
          mocha = require('gulp-mocha'),
          istanbul = require('gulp-istanbul'),
          clean = require('gulp-clean'),
          exec = require('gulp-exec'),
          plato = require('gulp-plato'),
          mversion = require('gulp-mversion');
          
This says to include GulpJS and all plugins associated with the tasks that we  will be building. Next, is task setup. The tasks in the preceeding gulpfile are:

* lint
* istanbul
* test
* test-coverage
* code-report
* reports
* clean-reports 
* dev 
* prod

All the tasks are pretty simple to read and for the sake of brevity, 
I will give an explanation of just the lint, dev and prod tasks.

**Lint Task**

    gulp.task('lint', function () {
    	gulp.src(['./lib/*', './test/*', './gulpfile.js'])
    		.pipe(jshint('.jshintrc'))
    		.pipe(jshint.reporter('default'));
    });
      
The lint task checks all JavaScript file(s) in the directories within the array and makes sure the code in the those directories meets a high standard.

**Dev Task**

    gulp.task('dev', function () {
    	gulp.run('lint', 'test');
    	gulp.watch(['./lib/*', './test/*'], function () {
    		gulp.run('lint', 'test');
		});
	});

The dev task runs the **lint** and **test** tasks then watches for file changes in the **lib** and **test** directories. As soon as a change is made and saved in either directory, gulp will then rerun the **lint** and **test** tasks.

**Prod Task**

	gulp.task('prod', function () {
        if (gulp.env.major) {
            gulp.run('lint', 'test');
            gulp.src('./package.json')
                .pipe(mversion('major'))
                .pipe(gulp.dest('./'));
        } else if (gulp.env.minor) {
            gulp.run('lint', 'test');
            gulp.src('./package.json')
                .pipe(mversion('minor'))
                .pipe(gulp.dest('./'));
        } else if (gulp.env.patch) {
            gulp.run('lint', 'test');
            gulp.src('./package.json')
                .pipe(mversion('patch'))
                .pipe(gulp.dest('./'));
        }
    });

The prod task looks pretty involved but it's actually not that bad. Let's say I am at the point where I'm ready to submit a pull request and I've made a minor change to the code base. From the command line I would run the following command: `gulp prod --minor`. And based on my **if statement** within the prod gulp task method (say that 3 times fast), if **gulp.env.minor** is true, in this case it is, then gulp runs the **lint** and **test** tasks. 

Once those tasks are done, gulp then takes the **package.json** file and pipes it to the **mversion** plugin which updates my project's minor version number by incrementing it by 1.

Pretty cool, huh?

### Conclusion

GulpJS is relatively young and the amount of plugins for it aren't quite as extensive as they are for GruntJS. But since plugin creation for gulp is pretty easy I expect to see a surge of available plugins in the near future.

With the examples I've provided I hope you will see the benefits in adding GulpJS to your next project and development workflow. If you have any questions be sure to post them in the comments below!

### Additional Resources

Here are some great follow up resources to help you go beyond the scope of the stuff I couldn't cover in this post.

* [GulpJS - The Streaming Build System](http://gulpjs.com)
* [GulpJS API docs](https://github.com/gulpjs/gulp/blob/master/docs/API.md)
* [GulpJS Plugins](https://npmjs.org/search?q=gulp)
* [Why you should use Streams](https://github.com/substack/stream-handbook)