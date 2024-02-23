## DataLad course material


# ⚠️⚠️⚠️ Development on future talks and workshop materials is continued in https://github.com/datalad-handbook/datalad-course ⚠️⚠️⚠️
(https://github.com/datalad-handbook/datalad-course was set up with a proper and more up-to-date reveal.js backbone that enables more reveal.js features and reliable PDF exports).


Talks and materials for workshops based on the [DataLad handbook](http://handbook.datalad.org).

**Slides** are written with [reveal.js](https://github.com/hakimel/reveal.js/) and can be found in ``talks/``.
PDFs of the slides are in ``talks/PDFs``.

**Casts** are written with [autorunrecord]() in the [book](https://github.com/datalad-handbook/book) itself. Finished casts can be found in ``casts/``. To find out how to create casts on your own machine, check out the [contributing instructions for the book for casts](http://handbook.datalad.org/en/latest/contributing.html#directives). Casts can be executed using the tool ``cast_live`` found in ``tools/``.

## Advice for creating presentations

- ``clone`` the repository to your local computer. From the root of the dataset, run ``git submodule update`` on relevant submodules. Those will be ``reveal.js``, and, if you want to access all images, ``pics/artwork`` Afterwards, you should be able to open the HTML's in a web browser and see them nicely rendered
- to generate a PDF from your slides, open the HTML of your talk in Chrome or Chromium, and append ``?print-pdf`` to the URL. Afterwards, you should be able to print to PDF from your browser. Alternatively, start an npm server and use ``decktape`` to generate it from HTML to PDF using ``docker run --rm -t --net=host -v `pwd`:/slides astefanutti/decktape http://localhost:8000 slides.pdf -s  1024x768``
- The tool [directpoll](https://directpoll.com/) works fantastic for virtual talks. See [#34](https://github.com/datalad-handbook/course/issues/34) for info on how to use it
- We have made good experiences with live code demonstrations. The ``tools/cast_live`` script is used for this. It is highly advised to test whether this script works on your set-up beforehand! You can write custom casts if you want to. Everything that's within a ``run '<code here>'`` statement is executed on ``Enter``, everything within a ``say '<note>'`` is written to your private terminal as a note.

## License

CC-BY-SA: You are free to

   - share - copy and redistribute the material in any medium or format
   - adapt - remix, transform, and build upon the material for any purpose, even commercially

under the following terms:

   - Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

   - ShareAlike — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.
