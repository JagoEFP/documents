# Migrating to Nuxt 3 from Nuxt 2

## Nuxt Bridge and why to not consider using it:

There is an intermediary step the Nuxt team offers developers who would like to update their Nuxt 2 apps to Nuxt 3 but are hesitant to make the leap as they wait for a stable release/are daunted by the task - Nuxt Bridge. "Nuxt Bridge is a compatibility layer that allows you to use Nuxt 3 features in Nuxt 2 with an opt-in mechanism." - \[1] The migration from Nuxt 2 to Bridge seems relatively straightforward, although there is next to no documentation for Nuxt Bridge to Nuxt 3, kinda leaving you high and dry. I see no reason to migrate to Nuxt Bridge, then puzzle out how to shift to Nuxt 3 when we could just hop straight to Nuxt 3, granted that being a greater undertaking but I believe more rewarding and less redundant use of time and resources to convert the app to an environment that seems to have stagnated and the developers left on hiatus as they continue to work on Nuxt 3.

## Resources

- \[1] https://nuxt.com/docs/migration/overview - migration guide on the main Nuxt website
- \[2] https://harlanzw.com/blog/nuxt-3-migration-cheatsheet - Cheatsheet, more like a step-by-step guide and deals a lot with upgrading modules \[Note: created in late 2022]
- \[3] https://debbie.codes/blog/migrating-nuxt2-nuxt3/ - Blog post, more a story to follow and take ideas from

## Differences

![[./nuxt_differences.png]]

### Code differences

- `nuxt.config` -> `defineNuxtConfig`
- `ajax` -> `fetch` API, `useFetch` composable
- `header` -> `useHead` composable
- `module.exports` -> `export default`
- `_ = require(...)` -> `import _ from ...`

## Dependencies/Possible issues

maybe consider switching package managers? Why use yarn (which we use currently), npm, pnpm (which seems to be a preferred one according to reddit) etc.

- Documentation for migration is not well documented
- Modules
	- **HAVE** to make sure that all modules being used have an upgrade path to Nuxt 3 compatibility/latest versions work with it/find a replacement module/the job of the module may even be redundant in Nuxt 3
	- Nuxt and Nuxt Modules are now build-time-only.
	- **Migration**
		1. Move all your `buildModules` into `modules`
		2. Check for Nuxt 3 compatibility of modules.
		3. If you have any local modules pointing to a directory you should update this to point to the entry file

### Our modules

- `eslint-config`: yes does have a nuxt 3 upgrade path
- `i8ln`: latest version is Nuxt 3 compatible


| Module Name | Current Ver. | Latest Ver. | Nuxt 3 | Notes |
| ---- | ---- | ---- | ---- | ---- |
| @ckeditor/ckeditor5-build-classic | 21.0.0 | 41.1.0 |  |  |
| @ckeditor/ckeditor5-vue | 1.0.3 | 5.1.0 |  |  |
| @ckeditor/ckeditor5-vue | 1.0.3 | 5.1.0 |  |  |
| @nuxt/content | 1.15.1 | 2.12.0 |  |  |
| @nuxtjs/i18n | 7.3.1 | 8.1.1 | Yes | v8 is compatible |
| @nuxtjs/robots | 2.5.0 | 3.0.0 | Yes | latest v does support Nuxt 3 |
| @nuxtjs/style-resources | 1.2.1 | 1.2.2 |  |  |
| apexcharts | 3.39.0 | 3.45.2 |  |  |
| axios | 0.27.2 | 1.6.7 | No | No longer recommended - `useFetch` API or `$fetch` |
| bootstrap | 4.6.2 | 5.3.2 |  |  |
| chart.js | 2.9.4 | 4.4.1 |  |  |
| chartist | 0.11.4 | 1.3.0 |  |  |
| config | 3.3.9 | 3.3.11 |  |  |
| echarts | 5.4.2 | 5.4.3 |  |  |
| fsevents | 2.3.2 | 2.3.3 | Yes? | Seems to have an issue with Vite |
| leaflet | 1.9.3 | 1.9.4 | ~Yes | New module called `nuxt3-leaflet` |
| nuxt | 2.16.3 | 3.10.2 | Yes | ...obviously |
| package.json | 0.0.0 | 2.0.1 |  |  |
| sass | 1.62.0 | 1.70.0 |  |  |
| sass-loader | 10.1.1 | 14.1.0 |  |  |
| sweetalert2 | 9.17.4 | 11.10.5 |  |  |
| vue | 2.7.14 | 3.4.19 |  |  |
| vue-chartist | 2.3.1 | 3.0.0 |  |  |
| vue-chartjs | 3.5.1 | 5.3.0 |  |  |
| vue-easy-lightbox | 0.14.1 | 1.17.0 |  |  |
| vue-echarts | 6.5.4 | 6.6.8 |  |  |
| vue-multiselect | 2.1.7 | 2.1.8 |  |  |
| vue-server-renderer | 2.7.14 | 2.7.16 |  |  |
| vuex | 3.6.2 | 4.1.0 |  |  |
| winston | 3.8.2 | 3.11.0 |  |  |

## Process

The main strategy seems to be to *create a brand new Nuxt 3 project* from scratch and then **copy** over code step by step so you can error correct and debug as you go. Trying to upgrade your project directly tends to lead to a mess of errors and dependency mismatches that seem like a maze to navigate.
