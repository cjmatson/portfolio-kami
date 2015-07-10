---
layout: post
title: BlocTime
thumbnail-path: "img/bloctime.png"
short-description: BlocTime is a time management application that focuses on work quality and productivity.
date: 2015-07-08

---

{:.center}
![]({{ site.baseurl }}/img/bloctime.png)

## Summary

In a 21st century world where procrastination is habitual and distrations are common due to social media, the Internet and new-age technology, work efficiency is a struggle. BlocTime was built to eliminate those hindrances so workers can produce satisfactory content and maintain production during work sessions. While it's significant for workers to stay on task during work sessions, it's also important to reward them with break sessions. BlocTime allows workers to take breaks in between work sessions to rest their mind and body from the labor. During those break sessions, it's recommended that workers also recap and review their work before beginning a new work session.

## Explanation

The creation of an application doesn't happen overnight. Construction takes time and effort because factors must be considered when building the application. BlocTime strives to achieve the following objectives in order to establish a simple, yet effective time management application:

- Determine the proper amount of time per work session, make sure it counts down to zero and transition to break
- Determine the proper amount of time per break session and make sure it counts down to zero and transition to work
- Create a sound that signals switch in sessions.
- Record the number of completed work sessions to create a longer break session
- Record the tasks and sync them into a database that compiles a task history
- Show completed tasks on the application 

## Solution

Before the application can be programmed, the index page needs to be established. The index page's head must include dependencies. The dependencies allow Firebase and AngularJS to work on the application. Firebase will be used to store the tasks, and AngularJS facilitates the programming. Once the dependencies are included, create a Javascript file that builds the application's module and controller. The Javascript file is where the application's code will be saved. The UI Router is injected into the module to generate multiple views within a single page, the index. The index is the foundational page for the entire application. The module subsequently produces a template for the home page, thus creating a controller called Home. The home page is where the content of the application will be stored. In the body of the index page, create a div element with the attribute "ui-view". This enables the application to display multiple template pages, or views. To confirm that the home template is showing in the application on the local server, test it by adding some HTML. Then, refresh the application on the local to check if the HTML appears. If it does, then you're ready to begin programming the application. 

An integral part of BlocTime is the tracker. The tracker is essentially a timer for the work and break sessions. To build a tracker, programming a directive is advisable. The tracker will not only feature the timer itself, but also the buttons that start and pause the sessions. The tracker directive's code will be featured in the application's javascript file, and its HTML will be located on its own template page. The tracker's HTML tags will be included in the home page.

The last major assignment in building BlocTime is showing, recording and syncing the tasks entered into the task history. In this assignment, Firebase and Home controller will be utilized. A task button will be created to enter the tasks into both Firebase and the application.

## Results

The bulk of the programming lies within the tracker directive. The tracker controls the time of the sessions and transitions between work and break. Below is a snippet of the implemented code that will be dissected for analysis:

{% highlight javascript %}
blocTime.directive('tracker',['$interval', function($interval) {
	return {
		restrict: 'E',
		templateUrl: '/templates/directives/tracker.html',
		scope: true,
		link: function(scope, element, attributes) {
			scope.mySound = new buzz.sound('http://soundjax.com/reddo/4122%5Edingdong.mp3', {
				preload: true,
			});
			scope.volume = 90;
			scope.playing = false;
			scope.watch = 1500;
			scope.buttonText = "Start Work";
			scope.breakButton = "Pause Break";
			scope.onBreak = false;
			scope.completedWorkSessions = 0;
			scope.countdown = function () {
				scope.watch--;
			}
			scope.$watch('watch', function(newValue, oldValue) {
				if (newValue === 0) {
					if (scope.onBreak) {
						scope.onBreak = false;
						scope.watch = 1500;
						scope.buttonText = "Start Work";
						scope.mySound.play();
						scope.mySound.setVolume(scope.volume);
						scope.playing = true;
					} else {
						scope.onBreak = true;
						scope.watch = 300;
						scope.breakButton = "Pause Break";
						scope.completedWorkSessions++;
						scope.mySound.play();
						scope.mySound.setVolume(scope.volume);
						scope.playing = true;
					}
					if (scope.completedWorkSessions === 4) {
						if (newValue === 0) {
							if (scope.onBreak) {
						scope.watch = 1800;
						scope.onBreak = true;
						scope.buttonText = "Start Break";
						scope.completedWorkSessions = 0;
						scope.mySound.play();
						scope.mySound.setVolume(scope.volume);
						scope.playing = true;
					        }
						else {
							scope.onBreak = false;
							scope.watch = 1500;
							scope.buttonText = "Start Work";
							scope.mySound.play();
							scope.mySound.setVolume(scope.volume);
							scope.playing = true;
							}
						}
					}
					scope.onBreakClicked();
				}
			});
			scope.buttonTextClicked = function () {
				if (scope.buttonText === "Start Work") {
					scope.buttonText = "Pause Work";
					scope.interval = $interval(scope.countdown, 1000);
				}
				else {
					scope.buttonText = "Start Work";
					$interval.cancel(scope.interval);
				}
			}
			scope.onBreakClicked = function () {
				if (scope.breakButton === "Pause Break") {
					scope.breakButton = "Start Break";
					$interval.cancel(scope.interval);
				}
				else {
					scope.breakButton = "Pause Break";
					scope.interval = $interval(scope.countdown, 1000);
				}
			}
		}
	}
}])
{% endhighlight %}

The $interval service must be inserted into the function of the directive. The $interval calls for a function. The second argument in the $interval is the delay. The delay, which is measure in milliseconds, determines how often the function is executed. The function necessary for the $interval service is a function that will decrement the timer in the sessions. That function is named scope.countdown(). When called, scope.countdown() decrements the time from "scope.watch", which is initially set at 1500, the start of a work session. 

But how is the scope.countdown() called? That is how the buttons are integrated. At the start of a work session, scope.onBreak is set to false to indicate that the it is not a break session. Scope.buttonText prints out "Start Work" on a button. When the button is clicked, the scope.buttonTextClicked() function is executed. The function uses a conditional statement to employ the scope.countdown(). If scope.buttonText equals "Start Work", scope.buttonText will change its text to "Pause Work" and the scope.interval will be called. Scope.interval is set equal to the $interval service that calls for the scope.countdown() function. However, if the button is clicked and scope.buttonText reads "Pause Work", then scope.buttonText will read "Start Work", and scope.interval will cancel. The timer will effectively pause the work session.

What happens when the timer equals 0 during a work session? In theory, the work session would transition into a break session, but if there was no additional code, the clock would continue to tick past zero and return a negative time. When the ticker hits zero, a $watch function is neeeded.

A $watch function is a function unique to AngularJS. It watches for any variable changes on a scope object. In this instance, the arguments in the function are newValue and oldValue. Similar to scope.buttonTextClicked() function, a conditional statement can be set. There are a several lines of that needed if the newValue of scope.watch equals zero because a multitude of requirements need to be met. 

First, the scope.breakButtonClicked() function needs to be employed. The scope.breakButtonClicked() is the function for the break button. The button should print "Start Break" and be set at 300. If the button is clicked, then it would print "Pause Break" and scope.countdown() would be executed. Similar to the other button, if the button is clicked when it reads "Pause Break", then it would print "Start Break", and the timer would pause.

But it's essential that scope.onBreak is set correctly for the two types of sessions. A conditional statement can be inserted inside the first conditional that asks if the newValue equals zero. Two scenarios will be featured in the conditional. Each of them represent a type of session. If scope.onBreak is false and scope.watch equals zero, then the application should switch to a break session (scope.onBreak equals true). During that switch, a sound will play, scope.mySound. The sound of scope.mySound is set to 90. When the application switches to a break session, it also records the number of completed work sessions, completedWorkSessions++. Scope.watch is set to 300 at the beginning of a break session. Once a break session finishes, the application switches back to a work session, scope.onBreak equals false. The same sound played at the end of a work session will also play once a break session concludes. 

The cycle continues until four work sessions have been completed. When four work sessions are recorded, then scope.watch will equal 1800 in a break session. However, the number of completed work sessions resets back to zero. In essence, an 1800-second break occurs once every four work sessions. 

But time isn't ready solely on seconds. Time needs to be read in minutes and seconds. To change time measurement, a filter is required.

{% highlight javascript %}
blocTime.filter('timecode', function() {
	return function(seconds) {
		seconds = Number.parseFloat(seconds);
		if (Number.isNaN(seconds)) {
			return '--:--';
		}
		var wholeSeconds = Math.floor(seconds);
		var minutes = Math.floor(wholeSeconds / 60);
		var remainingSeconds = wholeSeconds % 60;
		var output = minutes + ':';
		if (remainingSeconds < 10) {
			output += '0';
		}
		output += remainingSeconds;
		return output;
	}
	
})
{% endhighlight %}

The other component of the application is the entry of tasks. 

{% highlight javascript %}
blocTime.controller('Home.controller', ['$scope', '$firebase', function($scope, $firebase) {
	var ref = new Firebase("https://popping-heat-8018.firebaseio.com/");
	$scope.data = $firebase(ref);
	$scope.tasks = $scope.data.$asArray();
	$scope.task = {createdAt: Firebase.ServerValue.TIMESTAMP};
	$scope.addTask = function() {
		if ($scope.task.name) {
			$scope.tasks.$add($scope.task);
			$scope.task = {createdAt: Firebase.ServerValue.TIMESTAMP};
			return {
				all: $scope.tasks
			}
		}
	}
	$scope.returnTask = function($event) {
		if ($scope.task.name && $event.keyCode === 13) {
			$scope.tasks.$add($scope.task);
			$scope.task = {createdAt: Firebase.ServerValue.TIMESTAMP};
			return {
				all: $scope.tasks
			}
		}T=
	}
}])
{% endhighlight %}

Firebase is injected into the Home controller. The app created in Firebase, "https://popping-heat-8018.firebaseio.com/", will store all of the tasks entered into the input as an array. 

There are two ways to sync and display tasks. One way is to click on the "Add Task" button. That button corresponds with the $scope.addTask() function. The other way is to hit the "enter" key on the keyboard. The keyCode for the the "enter" key is 13. The function $scope.returnTask() corresponds with this event. 

The tasks are listed in reverse order, meaning the oldest tasks are shown toward the bottom. Firebase.ServerValue.TIMESTAMP records the time for the submitted task. 

## Conclusion

I was surprised how powerful a conditional statement could be in programming an application. My entire tracker directive was essentially done using conditional statements. I was also unaware that conditional statements can be stored inside conditional statements. Before I knew that, I was creating multiple functions that stored their own conditional statements. It simply didn't work. I even used different countdown functions for each of the sessions, but instead, I wisely used one for both of them.

I originally downloaded the Buzz ding sound, moved it into a folder and linked it to my Javascript file. However, it failed to work. Eventually, I copy and pasted the URL of the Buzz ding sound and employed it for scope.mySound.

This project gave me a better understanding of AngularJS, Firebase and even Javascript. Admittedly, I was overwhelmed when I began this project. It was the first one that I built from scratch. I used checkpoints and Bloc Jams from the foundational phase of my course as guidance for my work. I also researched and read online different aspects of AngularJS, Firebase and Javascript. Both resoruces helped immensely when constructing BlocTime. 
