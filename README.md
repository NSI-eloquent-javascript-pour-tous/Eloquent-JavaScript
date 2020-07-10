| Chapitre                               | Traducteur 1 | Traducteur 2 |
|----------------------------------------|--------------|--------------|
| 0. Introduction                        | efloti - fait   |              |
| 1. Values, Types, and Operators        | efloti - fait          |              |
| 2. Program Structure                   |              |              |
| 3. Functions                           |              |              |
| 4. Data Structures: Objects and Arrays | efloti - fait             |              |
| 5. Higher-order Functions              | efloti - fait  |              |
| 6. The Secret Life of Objects          | efloti             |              |
| 7. Project: A Robot                    |              |              |
| 8. Bugs and Errors                     |              |              |
| 9. Regular Expressions                 |              |              |
| 10. Modules                            |              |              |
| 11. Asynchronous Programming           |              |              |
| 12. Project: A Programming Language    |              |              |
| 13. JavaScript and the Browser         |              |              |
| 14. The Document Object Model          |              |              |
| 15. Handling Events                    |              |              |
| 16. Project: A Platform Game           |              |              |
| 17. Drawing on Canvas                  |              |              |
| 18. HTTP and Forms                     |              |              |
| 19. Project: A Pixel Art Editor        |              |              |
| 20. Node.js                            |              |              |
| 21. Project: Skill-Sharing Website     |              |              |

# Eloquent JavaScript

These are the sources used to build the third edition of Eloquent
JavaScript (https://eloquentjavascript.net).

Feedback welcome, in the form of issues and pull requests.

## Building

This builds the HTML output in `html/`, where `make` is GNU make:

    npm install
    make html

To build the PDF file (don't bother trying this unless you really need
it, since this list has probably bitrotted again and getting all this
set up is a pain):

    apt-get install texlive texlive-xetex fonts-inconsolata fonts-symbola texlive-lang-chinese inkscape
    make book.pdf

## Translating

Translations are very much welcome. The license this book is published
under allows non-commercial derivations, which includes open
translations. If you do one, let me know, and I'll add a link to the
website.

A note of caution though: This text consists of about 130 000 words,
the paper book is 400 pages. That's a lot of text, which will take a
lot of time to translate.

If that doesn't scare you off, the recommended way to go about a
translation is:

 - Fork this repository on GitHub.

 - Create an issue on the repository describing your plan to translate.

 - Translate the `.md` files in your fork. These are
   [CommonMark](https://commonmark.org/) formatted, with a few
   extensions. You may consider omitting the index terms (indicated
   with double parentheses and `{{index ...}}` syntax) from your
   translation, since that's mostly relevant for print output.

 - Publish somewhere online or ask me to host the result.

Doing this in public, and creating an issue that links to your work,
helps avoid wasted effort, where multiple people start a translation
to the same language (and possibly never finish it). (Since
translations have to retain the license, it is okay to pick up someone
else's translation and continue it, even when they have vanished from
the internet.)
