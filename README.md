# Using bindToController

## Overview

Up to now, the values passed through to our directives are attached to `$scope`. However, we've been taught to avoid `$scope` as much as we can. This is where `bindToController` comes in!

## Objectives

- Describe bindToController
- Declare bound properties
- Refactor scope property

## bindToController

`bindToController` works exactly like our `scope` property, but puts the items into `this` instead of `$scope`. This is awesome - as we're going to be using `controllerAs`, we should have all of our values assigned the same variable.

Let's take a look at how we'd convert our directives over to using `bindToController`:

### Before

```js
function TwitterCard() {
	return {
    		template: [
    			'<div class="twitter">',
    				'<a href="{{ twitter.link }}/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
    			'</div>'
    		].join(''),
    		scope: {
    		    handle: '='
    		},
    		controller: function ($scope) {
    			// $scope.handle === 'billgates'

    			this.twitterLink = 'https://twitter.com';
    		},
    		controllerAs: 'twitter',
    		restrict: 'E'
    	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

You'll notice the inconsistency - we're using `{{ handle }}` as well as `{{ twitter.link }}`.

### After

```html
<twitter-card handle="billgates"></twitter-card>
```

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="{{ twitter.link }}/{{ twitter.handle }}">Follow @{{ twitter.handle }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {},
		controller: function () {
			// this.handle === 'billgates'

			this.twitterLink = 'https://twitter.com';
		},
		controllerAs: 'twitter',
        bindToController: {
            handle: '='
        },
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

Order is restored. We now have consistency!

You might have noticed how we've still got our `scope` property - this is to tell Angular that we do still want a brand new scope - we're just not attaching anything to it.