SpreadCMS.js
============

SpreadCMS.js, gives you the ability to serve and manage dynamic content without the need of a dynamic server. It, creates a context from a Google Spreadsheet and renders it with a client side template engine (currently Mustache.js). 

Inspired by Timeline.js's workflow and Tabletop.js's Google Spreadsheet access.


Usage
-----

1. Create a Google spreadsheet to hold your data


2. Create an HTML file called `spreadcms.html` to look like this:

```
<script src="http://cdnjs.cloudflare.com/ajax/libs/hogan.js/2.0.0/hogan.js"></script>

<script type="text/javascript">
	var key = '[Spreadsheet public read access key]';
</script>

<script src="/spreadcms.min.js"></script>
```

3. Test

> python -m SimpleHTTPServer 8080


Deployment
----------

The main trick is to make the web server serve the `spreadcms.html` for every request that does not match a file, i.e. `Http 404`, and serve the file itself when it matches a template or a media file. That way, for every page request `spreadcms.html` will be loaded first and `spreadcms.js` then request and load relevant template and media files. 

To do this on AWS S3, for example, upload your files to S3 and setup your bucket as a static web site as usual, but set your `Index document` and `Error document` to point to `spreadcms.html`.


Template context
----------------

`spreadcms.js` creates a template context, depending on your spreadsheet structure, for the client-side template framework to consume.

```
{
  # sheet names
  models: [{name: sheet1}, {name: sheet2}, ...],
  
  # items as a dictionary to fetch items with id (row number)
  sheet1: {
             1: {attr1: attr1_row1, attr2: attr2_row1, ...},
             2: {attr1: attr1_row2, attr2: attr2_row2, ...},
             ...
           },
  sheet2: {
            1: {attr1: attr1_row1, attr2: attr2_row1, ...},
            2: {attr1: attr1_row2, attr2: attr2_row2, ...},
            ...
          },
  ...
   
  # items as an array to loop through the items  sheet1_list: [
               {attr1: attr1_row1, attr2: attr2_row1, ...}, 
               {attr1: attr1_row2, attr2: attr2_row2, ...}, 
                 ...
               ],
  sheet2_list: [
                 {attr1: attr1_row1, attr2: attr2_row1, ...}, 
                 {attr1: attr1_row2, attr2: attr2_row2, ...}, 
                 ...
               ],
  ...
}
```

The actual context created for the template can be view in the javascript console of the browser. 
