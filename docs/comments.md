# Contextual vs Literal Comments

## The 4 step process your code should go through

1. **Code Sketching:** You understand the problem, and how to solve it with code. Quickly write it out, don't waste time thinking about names, just focus on logic.

       var x = $(thing).height();
       if (x < 248) {
           $(otherThingy).css('width', 300);
       }

1. ***Literal* Comments:** Write a comment explaining your code

       //Get the height of the sidebar
       var x = $(thing).height();
       //If the sidebar height is less than 248px
       if (x < 248) {
           //Set the navigation in the sidebar to be 300px wide
           $(otherThingy).css('width', 300);
       }

1. **Refactor:** Use the language you wrote in your comments to make the code easier to read, replacing all the comments.

       var sidebarHeight = $('.sidebar').height();
       if (sidebarHeight < 248) {
           $('.sidebar nav').css('width', 300);
       }

   * *This is where many people say you should stop.* This is *acceptable* code. It is better than before, and once you've done this a few times you can skip step 2, and sometimes skip step 1 if you already have names in your head for these things and go straight to step 3. Everyone should, and will get to this point after some practice. **Commenting every line** is a good exercise because of this.

1. ***Contextual* Comments:** Add comments back in, this time explaining the *reasoning* behind this code existing.

       // We need to get the sidebar height because the UI relies on a 3rd party xyz module for our advertising that takes up height in the sidebar
       var sidebarHeight = $('.sidebar').height();
       // We specifically check this height because it's based on the default size of the advertising iframe from xyz. 248 is unique to xyz.
       if (sidebarHeight < 248) {
           // Because of xyz's requirements the nav needs to be between 280 & 345
           $('.sidebar nav').css('width', 300);
       }


1. **Refactor Comments:** Bake the contextual comments into the code with new variable names or functions

       function setSidebarWidthBasedOnXyzIframe () {
         var sidebarHeight = $('.sidebar').height();
         var defaultXyzAdvertisingIframeHeight = 248;
         if (sidebarHeight < defaultXyzAdvertisingIframeHeight) {
             // must be between 280 & 345
             var xyzNavWidth = 300;
             $('.sidebar nav').css('width', xyzNavWidth);
         }
       }
       setSidebarWidthBasedOnXyzIframe();

* * *

See, these comments tell me **a lot**.

* I know that `248` is specific and not to touch it.
* I know that `300` could be a range of numbers, and what they are in case I need to update this in the future. I won't need to rediscover that information.
* And most importantly I know that our company hasn't used *XYZ Advertising* for 2 years and that all of this code **can be removed**.

None of that information is conveyed if you stop at step 3. I don't know the weight of importance on the numbers that are used. What is/isn't safe to change. I don't know the reason behind *why* it was created to begin with.

Maybe you can convey some of this information by wrapping this in a function, or naming a file. But that should be determined by code style and project architecture, not because you want to avoid writing comments. If your project architecture permits or encourages this style of coding, and you can convey the information in the filename or something similar, that is fine, the point is that the contextual information must be conveyed *somewhere* in relation to the code itself.

* * *

### Summary

There are two types of comments, *literal* comments, which say what something *is*, and *contextual* comments, that explain *why* something is.

A literal comment is good for working out better variable/function naming. And should be used for that and then discarded.

A contextual comment gives context to your code, and supplies meta data that will be useful to those who are viewing this code for the first time. These should be used whenever possible. If there is supplemental information that is not conveyed, it should be documented with the code, in context.

All lines require literal comments, even if they are only mentally written before being discarded.

Not all lines require contextual comments. But anytime they *can* have them, they *must*. Otherwise you are doing a disservice to those who come after you.
