--- 
wordpress_id: PSD-13
layout: post
title: highlight a row in Google Spreadsheet
---

I'm in the midst of coming back from an amazing trip [around the world](http://blog.psdavey.com/2015/12/) and now
am in the process of getting a rental, buying a car etc. etc.

Anyway, my partner and I organise things through Google Spreadsheets, and
it's nice to be able to see who is assigned what with colour.

To do that in Google Spreadsheets

1. Make a status column (e.g who something is assigned to), use Data Validation on that cell.
2. Go to Format -> Conditional Formatting
3. In Apply to Range put in `A1:Y98` (which is a box from cell A1 to Y98, if you have more make the numbers larger)
4. Choose `Custom Formula Is` from the dropdown.
5. Insert `=INDIRECT("C"&Row())="Completed"` into the box. Now, in here, `C` is the column where my status cell is, and `Completed` is the value I am after
6. Choose your highlighting.

Bob is your veritable uncle.
