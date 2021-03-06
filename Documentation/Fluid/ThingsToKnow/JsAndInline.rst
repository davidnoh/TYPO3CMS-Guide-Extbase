.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

JavaScript and Fluid Inline Syntax
==================================

When Fluid ViewHelpers are used in mixed templates containing complex JavaScript blocks you might come across
the situation that a ViewHelper using inline syntax don't get interpreted.
In this case you can *wake up* the rendering by using a normal syntax ViewHelper or wrap the JavaScript part in
a CDATA block.

situation
---------

::

 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
 <html xmlns="http://www.w3.org/1999/xhtml">
 <head>
     <script src="http://code.jquery.com/jquery-1.10.1.min.js"></script>
     <script src="http://layout.jquery-dev.net/lib/js/jquery.layout-latest.js"></script>
     <script type="text/javascript">
         var myLayout;

         $(document).ready(function() {
             myLayout = $('body').layout({
                 north__size:                27
                 ,    north__initClosed:            false
                 ,    north__initHidden:            false
                 ,    center__maskContents:        true // IMPORTANT - enable iframe masking
             });
         });
     </script>

     <link rel="stylesheet" type="text/css" href="{f:uri.resource(path:'Css/main.css')}" />
 </head>
 <body>

 <div class="ui-layout-north">
     Some dumb text
 </div>
 <div class="ui-layout-center">
     Lorem ipsum
 </div>

 </body>
 </html>

The ViewHelper in line

::

 <link rel="stylesheet" type="text/css" href="{f:uri.resource(path:'Css/main.css')}" />

is not interpreted but ignored by fluid.

Solution
--------

- wake up Fluid rendering
  Introduce after the JavaScript block a normal syntax term, maybe::

  <f:commment>wake up, fluid!</f:comment>

- wrap the JavaScript part in CDATA::

   <![CDATA[
   ...
   ]]>

- use the :ref:`f:format.cdata <f-format-cdata>` viewHelper for the wrap::

   <f:format.cdata>
     <script type="text/javascript">
       var myLayout;
       $(document).ready(function() {
         myLayout = $('body').layout({
           north__size: 27,
           north__initClosed: false,
           north__initHidden: false,
           center__maskContents: true // IMPORTANT - enable iframe masking
         });
       });
     </script>
   </f:format.cdata>
