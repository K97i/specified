# Spec-ified v2
This is an in progress migration from PHP to a Svelte project.

# Development
## Architecture
The Svelte app uses turborepo to facilitate code re-use and splitting. 

`new/apps/web/`  contains the heart of the v2 web viewer. The entry
point for the app, `+page.svelte` is located at
`src/routes/viewer/[slug]/+page.svelte`.

*eventually*, `new/packages/ui` will contain re-usable Svelte components
that comprise the majority of the UI, with the long term goal of
creating a separate offline viewer.

`php/` contains the legacy viewer. As you add new components to the
Svelte app, you should remove them from the PHP side until the codebase
is entirely Svelte.

This architecture is messy, and subject to change. Cleanup is wanted.

## Prerequisites
You'll need:
- The latest stable version of node.js (and npm)
- PHP 8.1
- A test report at `php/files/test1.json`

### Setting up a test report
In the `php/` folder, create a folder named `files`. 

In this folder, place a valid spec-ify report named `test1.json`.

## Running
### Starting the PHP server
In one terminal, navigate to `php/` and run `php -S localhost:8080`. This will start a development server in the current directory on port 8080 (hardcoded for now).

### Installing dependencies
In another terminal window:

1. Navigate to `new/`, and run `npm install`
2. Navigate to `new/apps/web/`, and run `npm install`

### Running the Svelte app
1. From `new/apps/web/`, run `npm run dev`. You can also run `npm run dev` from `new/`, and it will start every app in the monorepo.
2. Navigate to `/viewer/test1` served from the URL provided by the previous command in your web browser, i.e `http://localhost:5713/viewer/test1`
