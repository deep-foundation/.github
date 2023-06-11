# Welcome to the Deep

# 1. Start a deep instance in [![Gitpod](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/deep-foundation/dev)

[![Start a deep instance in YouTube video](https://raw.githubusercontent.com/deep-foundation/.github/main/StartDeepInstanceInGitPod.png "Start a deep instance in YouTube video")](https://www.youtube.com/watch?v=MBZZDDASrqo)

# 2. Login as the administrator

[![Login as the administrator YouTube video](https://raw.githubusercontent.com/deep-foundation/.github/main/LoginAsAdministratorUserInDeep.png "Login as the administrator YouTube video")](https://www.youtube.com/watch?v=WNyz_jp9cAk)

# 3. Join our [![Discord](https://badgen.net/badge/icon/discord?icon=discord&label&color=purple)](https://discord.gg/deep-foundation) to get support or to become a Cadet of Depth

# 4. Use GPT-4 prompt to have complete control over your Deep instance:

### User prompt:

Deep is a system that uses Hasura on top of PostgreSQL. links table have columns: `id`, `type_id` (mandatory) , `from_id` , `to_id` (all with `bigint` type, `0` by default). We have `DeepClient` class in JavaScript, that usually placed in `deep` variable. It has `select`, `insert`, `update`, `delete` methods. Links array is returned as `data` field from these methods. `where` object is the first argument of select, update, delete methods, this object does not contain `where` field, this object also does not include `returning` field. `where` bool_exp has also `to`, `in`  and `from`, `out` relationships. `out` relationship is used for outgoing links, all `out` links have the same `from`/`from_id`. `in` relationship is used for incoming links, all `in` links have the same `to`/`to_id`. `table` and `returning` are set using options object, that is always a last argument of these methods. Only `returning` field is used to define which fields are returned by GraphQL queries/mutations. By default `table` field is `'links'`, and `returning` by default is set to `'id type_id from_id to_id value'`. If the default values of options are enough to get the code working we should not update them.

Each instance of `Package` link has incoming `PackageVersion` link from `PackageNamespace` link.

```js
const deletePackage = async(packageId) => {
  const packageTypeId = await deep.id('@deep-foundation/core' 'Package');
  const packageVersionTypeId = await deep.id('@deep-foundation/core' 'PackageVersion');
  // ...
  return await deep.delete({
    // ...
  });
}
```

@chatgpt fill blanks in the code. Use `_or`  in a single `deep.delete` call.

### GPT-4 response:

With the updated context and the requirement to use `_or` in a single `deep.delete` call, here is the revised implementation of the `deletePackage` function that deletes the `Package` link and its incoming `PackageVersion` link from the `PackageNamespace` link:

```js
const deletePackage = async (packageId) => {
  const packageTypeId = await deep.id('@deep-foundation/core', 'Package');
  const packageVersionTypeId = await deep.id('@deep-foundation/core', 'PackageVersion');

  // Delete the Package link and the PackageVersion link in a single deep.delete call
  const deletedLinks = await deep.delete({
    _or: [
      { to: { id: packageId }, type_id: packageVersionTypeId }, // Delete PackageVersion links
      { id: packageId, type_id: packageTypeId }, // Delete the Package link
    ],
  });

  return deletedLinks;
};
```

In this implementation, we use the `_or` property in the `where` object within a single `deep.delete` call to delete both the `Package` link and the `PackageVersion` links connected to the `PackageNamespace`. The result will be the deleted links, including both the `Package` and its incoming `PackageVersion` links.

# 5. Read [wiki](https://github.com/deep-foundation/documentation/wiki) for other life hacks.
