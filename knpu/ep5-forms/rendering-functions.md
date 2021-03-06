# Rendering Functions

Coming soon...

Right now, to render the form, we're just using a couple of very simple form
functions, one that makes the start tag, one that makes the tag and one that renders
everything else, so the problem is that we have almost zero control over how all of
this stuff renders how the actual html markup looks like, which is a problem when you
actually want to style this stuff more than just the normal bootstrap styling. Turns
out that this is actually one of the parts in the Symfony form system where things
get tricky. So let's start to talk about how to properly theme our forms. Go to your
other tab in Google for Symfony form rendering functions to find a page that talks
all about the functions that were already using inside of our. Instead of our
template like `form_start()` `form_end()` and a few others that we'll talk about. So first
`form_start()`. Yes, this just renders the form starts tag, which might seem kind of
silly, but that's what it does. You can as the is the `form_start()`. R, d does have a
second argument with an `array` of variables that can be passed to it

like method is that the method or an att are variable, which you can use to set any
other attributes on the form start tag, most commonly the class, the form tag,

well this one seems almost sillier because that's literally just the form closing
tag, but actually has one super important super power in that is it renders any form
fields that you may have forgotten to render. Now that might not make sense yet
because this magic form widget function is rendering all of our fields, but in a
second we're going to render our fields one by one and when we do that, if we've
forgotten to render any of the fields form end will render those fields and then the
closing form tag. Now, that may not seem like a good feature to you and it in many
ways it's not. The real point of this is that `form_end()` will render any hidden fields
automatically without you needing to do it. Most importantly, Symfonys CSRF token. So
I'll do inspect element again at the bottom of the forum, you'll see without us doing
anything, we have a hidden input tag, which is the CSRF token. When we submit that
Symfony automatically validates it. So basically, without doing anything, all of our
forms are protected by by a CSRF attacks. All right, so as far as the fields
themselves, the flexible way to run to them is `form_widget()`.

The next most if we're going to go, if you want just a little bit more control
instead of form widget, you can call `form_row()` and then render each
individual field like `articleForm.title`. I'll copy that and I'll paste it three
more times. Let's run. There are three other fields as well, so that's gonna be
`content` `publishedAt` and `author`,

so content published at

an author. Before we talk about that move over, refresh and got it. It looks exactly
the same. So the form, which I thing we had before, it's just a shortcut for calling
`form_row()` on each of your functions and `form_row()` is itself is something that is
capable of basically adding eight div around the field. Then rendering the label, the
widget itself, the help text, if there is any, and also the validation error so it
takes care of all of that, so this gives us a little bit more control, but we still
don't really have any control inside of there, but sometimes this is enough. Now if
you go and look at the `form_row` function, here we go. You'll notice that almost all
of these functions have something called an argument called `variables`. These
`variables` are explained a little bit at the bottom here where it talks about a number
of different variables that you can pass in for most fields, including a `label`, so if
you want to customize the `label`, we can now do that by adding a second argument 
`form_row()` in St, `label`

Article Title, some move over, refresh,

and boom article title changed. Now, the best way to. There's lots of different
things you can pass as form variables and it's actually the list is actually a little
bit different based on the field type. The best way to see what you can pass there
honestly, is to click back into the web debug toolbar for your form.

Click on the field you want to customize like `title` and down here, I'll collapse the
option stuff in addition to the options, which remember are the options that you're
passing as the third argument in your form class. There's also a section called view
variables. These are a bunch of variables that you're fueled, prepares and are used
when rendering the field and it gives you control over it pretty much every part of
the rendering process. So for example, you can see the `label` for this field. Notice
it's no, which basically means that it's no when we render our, our field and then
we're overriding it so you won't see your overwritten, uh, variables show up in here,
but they are working. Then you can see the health message and a whole bunch of other
things that help the form do its job, like the full name. That'll be the name
attribute. And even the ID. By the way, if it's useful, you can actually access these
directly in your template. I don't need to hear, but you could for example, um, print
`articleForm.title.vars.id`

and if you go back and look at your article, that'll actually print out the id of
that element which occasionally can be useful so you can override any of those
variables or even access them here and use them. Alright, so form road gets us a
little bit more flexibility, but if you want even more flexibility, there are a few
ways to do it, but one of them is to render all the fields of your form
independently. So I'm gonna get rid of the `form_row` instead. I'm going to render all
those parts individually. So I'm going to render `{{ form_label(articleForm.title) }}`, 
and in this case the second argument form a form label is actually the label.
Then `{{ form_errors(articleForm.title) }}`, any validation errors we have 
`{{ form_widget(articleForm.title }}`, that's the actual form field itself, and 
`{{ form_help(articleForm.title }}`. Those are the four things that the row renders 
internal life, but now go back and refresh. You noticed that looks basically the same 
though. If you kind of look a little bit more closely, you'll notice that all the other 
fields are wrapped in this forum group in this no longer is so when you do go, when 
you render things at this level, it's possible that you're going to start losing out 
on some special formatting that the `form_row()` gives you.

Another example is if I add the no validates attribute to the form and it enter, no,
I don't want to do that, nevermind. So for that reason I'm actually going to go back
to my form row and instead a little bit later, I'm going to teach you a way that you
can use form row, but customize how form row looks on this page only or even a across
your entire site using something called a `form_theme`. That's kind of the best of both
worlds because you can use the lazy way of doing things here, but still have a little
bit more control over how they look.