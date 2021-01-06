# Carta File Explorer Take-home

Welcome to the Carta front-end take-home challenge, we're excited to have you! 🎉🎉🎉

## Introduction

Today, you will be building a file explorer, similar to ones you're used to seeing in the left panel of your favorite code editor. File explorers typically show the directory structure of your project as a "tree" of folders and files nested within other folders.

Watch the `./user.interactions.mp4` video to see what the final product will look like.

## Setup

- Install Node and NPM
  - Mac
    - Install [Homebrew](https://docs.brew.sh/Installation)
    - Run `brew install node`
  - Windows
    - Download and run the [Node Windows installer](https://nodejs.org/dist/v14.15.3/node-v14.15.3-x86.msi)
  - Linux
    - See [instructions](https://nodejs.org/en/download/package-manager/) for your distribution's package manager
- Install Yarn
  - Run `npm install -g yarn`
- Run run `yarn` to install boilerplate dependencies

## Submitting

- Be sure to save any additional NPM dependencies to the `package.json` file
- Delete the `node_modules` folder
- Zip up the project directory
- Upload the Zip file to Google Drive (or other cloud storage)
- Create a sharable link and email it to your Carta recruiter

## Problem requirements

Your solution:

- **SHOULD**: Fetch the directory tree from the mock backend (see `src/api.js`). See API details below.
- **SHOULD**: Display all files in the project, with files nested inside of folders as needed.
- **SHOULD**: Allow folders to be collapsed and expanded.
- **SHOULD**: Display the appropriate icon next to each file. There is a "default file" icon () you can use as a fallback for unknown file types.
- **SHOULD**: When hovering over a file or folder, display a "delete" (`assets/icons/defaultFile.js`) button. Clicking on the delete button should delete the file or folder. You _must_ persist changes to the mock backend using the `removeBy(id)` API wrapper. See API details below.
- **SHOULD**: Try to match the provided mocks as closely as possible, time permitting. Prioritize functionality over visual fidelity. See detailed mocks below.
- **MAY**: Add unit or integration tests as needed, but we do not expect you to provide full test coverage.
- **MAY**: Use `lodash` or similar pure-JS utility libraries. Be sure to add them to the `package.json` file!
- **MAY**: Use CSS, SASS, or `styled-components` for styling
- **MAY**: Use TypeScript
- **SHOULD NOT**: Rely on third-party UI libraries, such as `semantic-ui-react` or `material-ui`. We want to see how _you_ style and design components!
- **SHOULD NOT**: Rely on third-party hook libraries. We want to see how _you_ write React code and manage state!

Finally, we don't want the exercise to take longer than a couple of hours. While you may choose to implement additional features, this is _neither required nor expected_, and will not impact how your solution is evaluated. We want you to focus on getting to a robust implementation of the core features!

## UI Mocks and Assets

Full UI:

- See `./mock.complete.png`

File hover states (default = top, hover = bottom):

- See `./mock.file.hover-states.png`

Folder hover states (default = top, hover = bottom):

- See `./mock.folder.hover-states.png`

Folder collapse states:

- See `./mock.folder.collapse-states.png`

Icons

- All icon assets can be found in the `./src/assets/icons` directory

## Mock backend API

You can import the mock API wrappers from `src/api.js`. There are only three methods:

### `getDirectoryTree(): Promise<DirectoryTree>`

Use this method to get a tree-like data structure that represents the project directory structure.

This is the data format:

```js
let directoryTree = {
  id: "[project id]",
  type: "project",
  name: "[project name]",
  children: [
    {
      id: "[folder id]",
      type: "folder",
      name: "[folder name]",
      children: [
        {
          id: "[folder id]",
          type: "folder",
          name: "[folder name]",
          children: [],
        },
        { id: "[file id]", type: "file", name: "[file name]" },
        // ...
      ],
    },
    { id: "[file id]", type: "file", name: "[file name]" },
    // ...
  ],
};
```

Rules:

- There are three types of nodes in a directory tree: `project`, `folder`, and `file`.
- Directory tree always starts with a `project` node.
- `project` and `folder` nodes have a `children: [...]` array:
  - `children` array can only contain `folder` or `file` nodes.
  - `children` array can be empty (e.g. empty project or empty folder).
- `file` nodes do not have children.
- Every node has a unique `id: UUID`.
- There is no limit to how deeply the tree can be nested.
- Inspect the `src/api.js` file to see the kind of data you will be rendering.

### `removeById(id: UUID): Promise<DirectoryTree>`

Use this method to remove a single file or folder by its node `id: UUID`. After removal, the method returns an updated directory tree.
