# A Cookie Recipe Guide to Class-Based Inheritance in Python 

## Introduction

I spent a long time just sort of understanding the surface level of inheritance in Python. Having a better understanding of manipulating inherited classes makes using external libraries or web frameworks like Django so much easier, and demystifies a lot of the magic of using other people's code. 

(Tip: Using PyCharm makes a lot of this a lot quicker and easier, too. You can Ctrl + click on the name of any class, and it will open a new tab to where that class is defined. It's easy to go down rabbit holes, but it's also easy to close the tabs and get back to what you were working on).


## Mother Class: Basic Cookie
Here we'll define a basic cookie.

```python
class Cookie:
	"""A basic, no-frills cookie"""
	name = 'sugar cookie'
	ingredients = ['flour', 'baking soda', 'baking powder', 'butter', 'white sugar', 'egg', 'vanilla extract']
	baking_temp = 375
	baking_time = 10
	mixins = None
	decorations = None
	
	def __str__(self):
		return self.name
		
	def make_cookies(self):
		print("Mixing up..." + (ingredient + '\n' for ingredient in self.ingredients))
		
	def baking_announcement(self):
		print("Cookies are baking at {0} for {1} minutes.".format(self.baking_temp, self.baking_time))
```
		
`Cookie` has a name, a list of ingredients (we'll leave off the proportions for simplicity's sake), and a temperature and time to bake. We'll define `mixins` and `decorations` as `None` for now, because a basic cookie needs no adornment.  

Let's bake a batch of basic `Cookie`s.
```console
>>> new_cookie = Cookie()
>>> 
>>> print("What's this? A " + str(new_cookie) + " recipe!")
What's this? A sugar cookie recipe!
>>> new_cookie.baking_announcement()
Cookies are baking at 375 for 10 minutes.
```

## Let's Get Modifying
Now, let's say you want to bake a batch of basic `Cookie`s, but just this once you're going to add green sugar on top, for St. Patrick's Day. You probably don't need a whole new recipe just for that small addition, and our original Cookie recipe has a holding space for decorations, so let's just add the decorations to our one-time batch of cookies:

```console
>>> st_pattys_day_cookie = Cookie()
>>> st_pattys_day_cookie.decorations = ['green sugar']
>>> 
>>> print("What's this? A " + str(new_cookie) + " recipe!")
What's this? A sugar cookie recipe!
>>> new_cookie.baking_announcement()
Cookies are baking at 375 for 10 minutes.

```

As you'll see, the `str()` and `baking_announcement()` methods return the same output as before, because at its heart, this is really the same kind of `Cookie` with a very small tweak for a specific situation. It can be more of an art than a science to determine whether you want to create a new class that inherits from the old class, or make small tweaks to the old class. 

In general, if the old class has all of the attributes you already need in your new class, but you just want to _change_ them, you can use the old class. If you need something fundamentally different in the new class, or you want to change a _method_, you're better off inheriting.

## Child Cookie #1
Let's use another example. You want to make your St. Patrick's Day `Cookie` again with the green sugar, but you want to call it a St. Patrick's Day sugar cookie and add a holiday message to the `baking_announcement()` method. You want to keep everything else the same, though, so instead of repeating every line of the original `Cookie` class and changing the parts you want, which would look like this:

```python
class StPatricksDayCookie:
	"""A St. Patrick's Day sugar cookie"""
	name = 'St. Patrick's Day sugar cookie'
	ingredients = ['flour', 'baking soda', 'baking powder', 'butter', 'white sugar', 'egg', 'vanilla extract']
	baking_temp = 375
	baking_time = 10
	mixins = None
	decorations = ['green sugar']
	
	def __str__(self):
		return self.name
		
	def make_cookies(self):
		print("Mixing up..." + (ingredient + '\n' for ingredient in self.ingredients))
		
	def baking_announcement(self):
		print("Cookies are baking at {0} for {1} minutes.".format(self.baking_temp, self.baking_time))
		print("Happy St. Patrick's Day!")
```

You can save several lines of code and some time by inheriting from the `Cookie` class and only modifying the parts that will be different:

```python
class StPatricksDayCookie(Cookie):
	"""A St. Patrick's Day sugar cookie"""
	name = 'St. Patrick\'s Day sugar cookie'
	decorations = ['green sugar']
	
	def baking_announcement(self):
		super().baking_announcement()
		print("Happy St. Patrick's Day!")
```		
		
See how this only explicitly writes out the parts you want to _change_? Imaging that everything from `Cookie` is still available to you, but it's just **invisible** now. To see through the invisibility shield, you have to go to the parent (which, again, you can do by Ctrl + clicking in PyCharm).

---
#### A Special Note
`super()` can be confusing to even advanced Python users, so don't sweat it. It basically replaces the name of the parent class. Here, `super().baking_announcement()` means the same thing as `Cookie.baking_announcement()`, and it means "for this method, go copy everything from my parent's method before you add this last new piece at the end."  

##### Zippy Summary:
* `super()` just means "whatever my parent is." 
---

Try to predict what will happen with the following code, and then run it in your favorite Python interpreter.

```python
st_pattys_day_cookie = StPatricksDayCookie()
print("What's this? A " + str(new_cookie) + " recipe!")
print(st_pattys_day_cookie.baking_announcement())
```
