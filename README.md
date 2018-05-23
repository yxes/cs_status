# cs_status
_dynamically displays open/closed to the web based on customer service hours_

## INSTALLATION

None really. Just open the [index.html](http://htmlpreview.github.io/?https://github.com/yxes/cs_status/blob/master/index.html) file in your browser. It's all
self-contained.

## SYNOPSIS

Our customer service hours are posted on our website in the ET timezone. This
is an effort to have our customers see whether or not we are open. It sounds
very simple but it requires some timestamp conversions to figure out and this
takes care of doing that for you.

All the CSS and JS is imbedded in the page. Also note that I'm using ES6 here
so you may want to babel it if you care about such things.

### CSS

The CSS is linked to the `open_closed.jpg` file and as such requires the frame 
move down exactly 55px to display the _closed_ message. In other words, you'll 
want to play around with the window heights if you change out the image.

### JS

**NOTE:** This is all found [here](index.html) and you really don't need to know any of this to use it. 

If you want to make radical changes though, here's exactly how the JS works. If you list your default open / closed times 
in another timezone you'll want to update the offset and if you don't bother
with daylight savings (_you lucky dog_) then you can remove the 
`Date.prototype` code entirely.

*STEPS*
1. setup our hours in 24 hour time
  * these are just the start and stop times for each day
  * you'd have to modify this a bit to handle more refined times 
    (i.e. we close @5:30pm) because it's currently only to the hour
2. using the current date gather up the `start_hour` and `end_hour` from our
configuration variable (`hours`)
3. See if we are within daylight savings.
  * Look at `jan`uary and `jul`y and find the largest timezone offset.
Assign this to the `Date()` object as `stdTimezoneOffset`
  * Return `true` or `false` if `isDstObserved` is called from a `Date()` obj.
4. If we are in daylight savings then subtract one from the `start_hour` and
`end_hour` variables. (if your default timezone doesn't support daylight
savings, for instance you live in the USA in the state AZ or IN, skip this)
5. Since we added to the `end_hour` it could be possible to move it up to or
past the 24 hour max. In that case, just subtract 24 hours.
6. We define a function (`set_time`) that creates a new date and modifies the
hour. We'll use this to set the stop / start times.
  * note that we need to ensure that the date doesn't move ahead to the next
day. We ensure it won't by setting the current date as our last call.
7. Set our start and end times based on the new hours (now in UTC) we 
calculated earlier.
8. It's possible that because we moved our `end_hour` back by 24 hours that we
moved our new `end_time` before our `start_time` so we just add a day if that's
the case.

That's about it. When the page opens we call our function and then set and 
Interval to update it every minute. Once it changes our `<div>` elment gets a
new class. (either `cs_open` or `cs_closed`).

## LICENCE & COPYRIGHT

This program is free software; you can redistribute it and/or modify it however
you please. Author assumes no responsibility.

The contents of this repository are covered under the [MIT License](LICENSE).

Copyright &copy; 2018. Steve Wells. All rights reserved.

## AUTHOR

[Steve Wells](https://www.stephendwells.com/)







