---
layout: default
title:  "Import & Export"
date:   2018-03-04
---

BITBOX recently underwent a [large code refactor](https://github.com/bigearth/bitbox-electron/pull/10/files) to port to [Redux](https://redux.js.org). This made us completely think out the state tree, remove any calls to `this.setState()`, create action types/creators and compose reducers w/ `combineReducers` and `<Provider>`. This change is completely under the hood and won't affect the way BITBOX behaves nore will it introduce any breakages w/ your apps.

This change is to ensure that as BITBOX grows there is clarity around how things are happening internally. With that in mind you can now export the state of your BITBOX and import it in to another instance of BITBOX. This should make it much easier to debug what's happening w/ your app state as well as introduce regular backups.

## Export

![Import and Export buttons]({{ "/assets/import-export.png" | absolute_url }})

Now at the top right next to the configuration cog icon you'll see import and export icons. If you click the Export icon to open a modal with the entire state of your BITBOX as Javascript Object.

![Export 1]({{ "/assets/export1.png" | absolute_url }})

You can click 'copy to clipboard' and your BITBOX state will be copied. You're able to share this state w/ anyone and they can import it in to their BITBOX and help you debug.

![Export 2]({{ "/assets/export2.png" | absolute_url }})

## Import

If you click the Import button a modal will open with `<textarea>` in to which you can paste the state of a BITBOX and it will import it. This can be very useful for restoring regular back ups or importing a BITBOX to help debug.

![Import 1]({{ "/assets/import1.png" | absolute_url }})

After pasting in the BITBOX you which to import click the 'import' button and the BITBOX was properly formatted you'll see 'imported' with a checkbox. Your local BITBOX should now reflect the BITBOX which you just imported.

![Import 2]({{ "/assets/import2.png" | absolute_url }})

## Summary

Importing and Exporting your BITBOX allows you to more easly make backups as well as share the state of your BITBOX for debugging.
