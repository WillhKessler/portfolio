---
title: "Creating a 'Library of Things' With Free and Opensource Tools"
excerpt: "Using Github Pages and Google Sheets to create a simple 'Library of Things'"
classes: wide
header:
  overlay_color: "#bf5ebf"
  image: /assets/images/LibraryOfThings.png
  teaser: /assets/images/LibraryOfThings.png
sidebar:
  - title: "Tools and Techniques"
    text: "\r\n
    * Github Pages: Website hosting platform \r\n
    * Google Suite: Sheets and Forms \r\n
    * JS, CSS: Site formatting" 
---
In this tutorial we will be utilizing Github Pages to host a simple 'Library of Things' that small organizations or individuals can use to lend items. This simple site utilizes a spreadsheet hosted in Google Sheets to catalog all the items with unique IDs, item names, descriptions, and other details. The GitHub pages website displays the contents of the sheet as a nicely formatted table and displays a simple checkout form that users can populate with items from the library. Requesting the items is passed to a Google Form which writes the requests to another page in the Library spreadsheet, and sends an email to the sheet owner when a new request is placed. 

In this example, we will use the 'North Cascades Gear Library' which is a free Library of Things for bikepacking and camping equipment based in Bellingham, WA. 

The most complicated part of this tutorial for the lay person is likely going to be building comfort with Github. The platform has a plethora of features that we won't touch on here, but think of it simply as as the place that your website, and the simple code that powers it lives. 

# Catalog your 'Library of Things'
Use [Google Sheets](https://docs.google.com/spreadsheets/create) to create a catalog of your items. 
Create a new Google sheet document using the Google account of the person managing the Library. It does not need to be the same person who owns the Github account hosting the site. 
A simple catalog should have at minimum the following columns:
1. ID- a unique id for each item
2. Item- the Name of the item
3. Category- if your library of things has multiple types of items, create broad categories describing each type.
4. Subcategory- if your library categories can be broken down further, utilize a subcategory column.
5. Type- type can further refine the subcategories of items
6. Brand- Brand of the item if pertinent
7. Model- model name of the item, if pertinent
8. Fitment- 
9. Dimensions-
10. Weight (g)- 
11. Condition- The condition of the item, this can be important for tracking the wear of an item and what condition it was in before it was borrowed.
12. Status- This column is crucial for displaying whether an item is available for borrowing
13. Due Back- A column indicating the expected return date of the item
14. Notes- a broad field for including additional information about the item
15. photos- a photo for a photo album link, an image hosting site works best.

Additional Google Sheets automations can be applied as necessary to build out the usability of the catalog for the maintainer. For example, apply conditional formatting to a row based on the value in the 'Status' Column. If the item is available, color the column green, if it's checked out, color it red, and if it's unavailable for some other reason, grey out the column and text. This customization won't be visible in the finished website, it's solely for back-end purposes. 



# Set up your GitHub Pages website: 
If you don't have a Github account, create one.
## Create a Library of Things Repo
Github organizes the files and processes for individual projects into separate directories called repositories, or 'repos' for short. You'll need a repo for your Library of things.
Rather than create all the contents of our Library site from scratch, we are going to copy the layout and content from an existing library (the North Cascades Gear Library). The act of copying an entire repo into your own Github account is called 'Forking'. 
1. Navigate to the North Cascades Gear Library using the search box at the top of the page. Enter 'northcascades_gearlibrary' and select the appropriate result.
2. 'Fork' the Github repo for the North Cascades Gear Library: We'll discuss the inner workings, but for simplicity's sake copying the entire repo is simpler than starting from scratch. 

# Set up your Lending Request Form: 
Use Google Forms to create a simple request form utilizing the same google account that owns the Library of Things Google Sheet. 
We'll need the following fields to create a simple request form.
1. Name- Short Answer Text Field, library users will enter their full name 
2. Email- Short Answer Text Field, library users will enter a valid email that the library will use to contact users to set up a time to retrieve their requested items.
3. Desired Pick Up Date- Date Field, library users will enter the date they'd like to pick up their requested item(s)
4. Desired Return Date- Date Field, library users will enter the date they'd like to return their requested item(s). Library policies can vary, so you may want a set borrowing period
5. Notes- Long Answer Text, library users can enter any additional information
6. Requested Items- Long Answer Text, this field is automatically populated when the form is submitted based on the items the user requests to borrow.

# Optional: Set up web hosting for a custom web address for your library.
## Step 1



# Usage

