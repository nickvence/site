.. title: Using reST
.. slug: using-rest
.. date: 2020-12-10 11:48:49 UTC-05:00
.. tags: R, covid, ggplot
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: true

Let's put reST through the paces -- to see how the features work.
I want to do the following:

* Insert picture
* Use math: :math:`E=mc^2`
* Use hyperlink
* use cross-refference

Here COVID deaths vs time compared to previous mortality.

.. image:: /images/bar_chart.png
    :width: 600

This data comes from the `CDC`_. 


.. include:: listings/bar_chart_COVID.R
    :code: R

Copying and pasting code project code makes for a fragile site.  The following steps  maximize reproducibility:

- Clean and comment the original source.
- Create a soft link :code:`ln -s path/to/code.py` in :math:`listings/`
- Use the reST :code:`.. include` directive inserts the code directly from the file.
  The :code:`:code: R` option provides syntax highlighitng.
- Unfortunately, RStudio and HTML may have different tabstops (number of spaces in a tab) causing an indentation mismatch.

.. _CDC: http://data.cdc.gov/NCHS/Weekly-Counts-of-Death-by-State-and-Select-Causes/muzy-jte6
