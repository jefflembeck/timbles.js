timbles.js
==========

*timbles* is a very lightweight jQuery plugin (lol is that an oxymoron) that allows you to add sorting to an existing `<table>`. That's it. Actually, it can also create the table for you with some JSON data and minimal configuration. It's p easy. I think it would make for a great base for table functionality you'd want to build into your apps.

## Install

You need to enqueue jQuery (duh) and the *timbles.js* file.

<pre><code>&lt;script src="jquery.js">&lt;/script>
&lt;script src="timbles.js">&lt;/script></code></pre>

## Configure *timbles*

### Existing Table of data

Let's say you have a table on your page already, all populated with data:

<table border="1">
  <thead>
    <tr>
      <th id="name">Name</th>
      <th id="size">Size</th>
      <th id="kind">Kind</th>
      <th id="date-added">Date Added</th>
      <th id="notes" class="no-sort">Notes</th>
    </tr>
  </thead>
  <tr>
    <td>dhtmlconf.png</td>
    <td>77 KB</td>
    <td>PNG Image</td>
    <td>August 31, 2014, 11:16 PM</td>
    <td>dhtmlconf logo</td>
  </tr>      
  <tr>
    <td>icla.pdf</td>
    <td>26 KB</td>
    <td>Adobe PDF document</td>
    <td>August 27, 2014, 12:51 PM</td>
    <td>Individual Contributor License Agreement</td>
  </tr>
  <tr>
    <td>Slime Girls - Vacation Wasteland EP.zip</td>
    <td>72.9 MB</td>
    <td>ZIP archive</td>
    <td>August 25, 2014, 9:40 PM</td>
    <td>cool chiptunes from lwlvl</td>
  </tr>
</table>


And here is the source code:


<pre><code>&lt;table border="1">
  &lt;thead>
    &lt;tr>
      &lt;th id="name">Name&lt;/th>
      &lt;th id="size">Size&lt;/th>
      &lt;th id="kind">Kind&lt;/th>
      &lt;th id="date-added">Date Added&lt;/th>
      &lt;th id="notes" class="no-sort">Notes&lt;/th>
    &lt;/tr>
  &lt;/thead>
  &lt;tr>
    &lt;td>dhtmlconf.png&lt;/td>
    &lt;td>77 KB&lt;/td>
    &lt;td>PNG Image&lt;/td>
    &lt;td>August 31, 2014, 11:16 PM&lt;/td>
    &lt;td>dhtmlconf logo&lt;/td>
  &lt;/tr>      
  &lt;tr>
    &lt;td>icla.pdf&lt;/td>
    &lt;td>26 KB&lt;/td>
    &lt;td>Adobe PDF document&lt;/td>
    &lt;td>August 27, 2014, 12:51 PM&lt;/td>
    &lt;td>Individual Contributor License Agreement&lt;/td>
  &lt;/tr>
  &lt;tr>
    &lt;td>Slime Girls - Vacation Wasteland EP.zip&lt;/td>
    &lt;td>72.9 MB&lt;/td>
    &lt;td>ZIP archive&lt;/td>
    &lt;td>August 25, 2014, 9:40 PM&lt;/td>
    &lt;td>cool chiptunes from lwlvl&lt;/td>
  &lt;/tr>
&lt;/table></code></pre>

You don't need to generate a new table with JSON because your table is there already. And you just want to be able to make it sortable. Then just call this after you enqueued *timbles.js* as per the above install directions:

<pre><code>// get your table
var $table = $('table');

// call timbles
$table.timbles({ sorting: true });</code></pre>

In order for sorting to work, the `<th>` cells need to have an `id` attribute and the parent `<tr>` of the header rows needs to be within a `<thead>` tag. Again, view the source above for the example.

If you want to initially sort your table on load, there are `sorting` properties you can set:

<pre><code>// get your table
var $table = $('table');

// call timbles with sorting property
$table.timbles(
  sorting: {
    order: 'asc', // 'asc' for ascending sort, 'desc' for descending
    keyId: 'name', // the id of the column you want to initially sort by
  }
});</code></pre>

If you don't want all of the columns to be sortable, add the class `no-sort` to the &lt;th> element of the column you do not want to be sortable.

### Generating a table from a JSON file or an array of row objects

If your data is in a JSON file or array, add a `<table>` element to your page and then call `timbles` on it.

<pre><code>&lt;table id="example">&lt;/table></code></pre>

<pre><code>// get your table
var $table = $('#example');

// call timbles with dataConfig property
$table.timbles({

  dataConfig: {
    
      /**
      * There are two types of data that timbles currently accepts:
      * - a json filename
      * - an array of row objects
      * 
      * by default, timbles will look for a json file name, 
      * so dataType is not required unless you are giving an array.
      * Then you will need to set it to 'array'.
      */
      
    dataType: 'json', // right now data types can be 'array' or it defaults to 'json'
    data: 'data.json', // the json file if dataType is 'json', an array if dataType is 'array'
    columns: [
      
      /**
      * you have to set the column headers with the following required properties:
      * - label (string), the text between &lt;th> and &lt;/th>
      * - id (string), the json object property attributed to the column
      * 
      * and here are some optional properties:
      * - noSorting (boolean), if set to true the column won't be sortable 
      *   if you have the non-column property sorting set to true
      * - dataFilter (function), this function will be applied to the cell value, 
      *   useful for when you want to add currency symbols, html, etc
      */

      { label: 'Name', id: 'name' },
      { label: 'Size', id: 'size' },
      { label: 'Kind', id: 'kind' },
      { label: 'Date Added', id: 'dateAdded' },
      { label: 'Notes', id: 'notes', noSorting: true }
      { label: 'Price', id: 'price', dataFilter: function(value) { return '$' + value; } }
    ]
  }
  sorting: true, // if you want columns to be sortable

});</code></pre>

Here is an example of an array of row objects:

<pre><code>var localData = [
  {
    name: "dhtmlconf.png",
    size: "77 KB",
    kind: "PNG Image",
    dateAdded: "August 31, 2014, 11:16 PM",
    notes: "dhtmlconf logo"
  },
  {
    name: "icla.pdf",
    size: "26 KB",
    kind: "Adobe PDF document",
    dateAdded: "August 27, 2014, 12:51 PM",
    notes: "Individual Contributor License Agreement"
  },{
    name: "Slime Girls - Vacation Wasteland EP.zip",
    size: "72.9 MB",
    kind: "ZIP archive",
    dateAdded: "August 25, 2014, 9:40 PM",
    notes: "cool chiptunes from lwlvl"
  },{
    name: ".DS_Store",
    size: "25 KB",
    kind: "Virus",
    dateAdded: "February 7, 2014, 10:59 PM",
    notes: "lol"
  },{
    name: "No_Diggity.mid",
    size: "17 KB",
    kind: "MIDI file",
    dateAdded: "July 3, 2014, 11:34 PM",
    notes: "very important karaoke file"
  },{
    name: "jorts.svg",
    size: "52 KB",
    kind: "Plain Text File",
    dateAdded: "May 24, 2014, 1:55 PM",
    notes: "logo for jort.technology"
  },{
    name: "wordpress.sql",
    size: "418 KB",
    kind: "Plain Text File",
    dateAdded: "May 11, 2014, 11:15 PM",
    notes: "blog dump"
  },{
    name: "foundation-compass-template-master.zip",
    size: "6 KB",
    kind: "ZIP archive",
    dateAdded: "April 21, 2014, 6:59 PM",
    notes: "c.s.s. is better that javascript"
  }
];</code></pre>

## C.S.S.

When you sort ascending, the `sort-asc` class is added to the `<th>` header. If you sort descending, the `sort-desc` class is added to the `<th>` header. So you can, like, use CSS to add arrows whenever those classes are set and that's p cool I think.

## Note

This was made within a few days for jqCon Chicago on 9/12/2014 and therefore is incomplete and not documented as well as I'd like. This is what I get for having to launch something whenever I give a talk. I'll refine this and add more features while retaining the philosophy of staying simple. 

**MOST IMPORTANTLY: HAVE FUN.**