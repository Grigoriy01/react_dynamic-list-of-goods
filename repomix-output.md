This file is a merged representation of a subset of the codebase, containing specifically included files, combined into a single document by Repomix.

<file_summary>
This section contains a summary of this file.

<purpose>
This file contains a packed representation of a subset of the repository's contents that is considered the most important context.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.
</purpose>

<file_format>
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Repository files (if enabled)
5. Multiple file entries, each consisting of:
  - File path as an attribute
  - Full contents of the file
</file_format>

<usage_guidelines>
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.
</usage_guidelines>

<notes>
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Only files matching these patterns are included: src/**/*, README.md
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Files are sorted by Git change count (files with more changes are at the bottom)
</notes>

</file_summary>

<directory_structure>
src/
  api/
    goods.ts
  types/
    Good.ts
  App.scss
  App.tsx
  GoodsList.tsx
  index.tsx
  vite-env.d.ts
README.md
</directory_structure>

<files>
This section contains the contents of the repository's files.

<file path="src/types/Good.ts">
export interface Good {
  id: number;
  name: string;
  color: string;
}
</file>

<file path="src/App.scss">
// styles go here
</file>

<file path="src/vite-env.d.ts">
/// <reference types="vite/client" />
</file>

<file path="src/GoodsList.tsx">
import React from 'react';
import { Good } from './types/Good';

type Props = {
  goods: Good[];
};

export const GoodsList: React.FC<Props> = ({ goods }) => (
  <ul>
    {goods.map(good => (
      <li key={good.id} data-cy="good" style={{ color: good.color }}>
        {good.name}
      </li>
    ))}
  </ul>
);
</file>

<file path="src/api/goods.ts">
import { Good } from '../types/Good';

// eslint-disable-next-line
const API_URL = `https://mate-academy.github.io/react_dynamic-list-of-goods/goods.json`;

export function getAll(): Promise<Good[]> {
  return fetch(API_URL)
    .then(response => {
      if(!response.ok) {
        throw new Error('Failed to fetch users')
      }

      return response.json()
    })
    .then(data => {

      return data;
    })
}

// sort and get the first 5
export const get5First = () => {
  return getAll()
    .then(goods => {
      const sortGoogs  = [...goods].sort((a, b) => a.name.localeCompare(b.name));

      return sortGoogs.slice(0, 5)
    });
};

// get only red
export const getRedGoods = () => {
  return getAll()
    .then(goods => {
      const filterGoogs = goods.filter(el => el.color === 'red');

      return filterGoogs;
    });
};
</file>

<file path="src/App.tsx">
import React, { useState } from 'react';
import './App.scss';
import { GoodsList } from './GoodsList';

import { getAll, get5First, getRedGoods } from './api/goods';
import { Good } from './types/Good';
// or
// import * as goodsAPI from './api/goods';

export const App: React.FC = () => {
  const [selectedGoods, setSelectedGoods] = useState<Good[]>([]);

  return (
    <div className="App">
      <h1>Dynamic list of Goods</h1>

      <button
        type="button"
        data-cy="all-button"
        onClick={() => getAll().then(data => setSelectedGoods(data))}
      >
        Load all goods
      </button>

      <button
        type="button"
        data-cy="first-five-button"
        onClick={() => get5First().then(data => setSelectedGoods(data))}
      >
        Load 5 first goods
      </button>

      <button
        type="button"
        data-cy="red-button"
        onClick={() => getRedGoods().then(data => setSelectedGoods(data))}
      >
        Load red goods
      </button>

      <GoodsList goods={selectedGoods} />
    </div>
  );
};
</file>

<file path="src/index.tsx">
import { createRoot } from 'react-dom/client';
import { App } from './App';

createRoot(document.getElementById('root') as HTMLElement).render(<App />);
</file>

<file path="README.md">
# React dynamic list of goods

> Here is [the working page](https://mate-academy.github.io/react_dynamic-list-of-goods/)

You have 3 button that should load [the goods](https://mate-academy.github.io/react_dynamic-list-of-goods/goods.json) and show them on the page using the `GoodsList`.

1. `Load All goods` should load and show all the `goods`;
1. `Load 5 first goods` should do the next:
    - load all the goods;
    - sort them by name;
    - and show the first 5;
1. `Load red goods` should load all the goods show only `red` ones;
1. Server has only 1 endpoint returning all the goods, so you should do all the preparations in corresponding methods in `/api/goods`.
1. `GoodsList` is almost finished, you just need to use corresponding colors for `li`s;

## Instructions
- Install Prettier Extention and use this [VSCode settings](https://mate-academy.github.io/fe-program/tools/vscode/settings.json) to enable format on save.
- Implement a solution following the [React task guideline](https://github.com/mate-academy/react_task-guideline#react-tasks-guideline).
- Use the [React TypeScript cheat sheet](https://mate-academy.github.io/fe-program/js/extra/react-typescript).
- Open one more terminal and run tests with `npm test` to ensure your solution is correct.
- Replace `<your_account>` with your Github username in the [DEMO LINK](https://<your_account>.github.io/react_dynamic-list-of-goods/) and add it to the PR description.
</file>

</files>
