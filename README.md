HLB module library
---

This repository aims to solve the same issue as the [Docker Official Images](https://github.com/docker-library/official-images) but for the `Dockerfile` space. We want to provide high quality modules solving common workflows that you can import into your HLB program.

## Goals

1. Bootstrap the ecosystem with high quality modules
2. Prescribe best practices for HLB

Perhaps in the future, we can work with the upstream project (or with the blessing of the upstream project) to create HLB modules that are true to the upstream project's vision for how their project is intended to be consumed.

## Contributing

1. Create a directory with the identifier you expect users to import the module as.

Example: 
```hlb
import docker from fs {
	image "openllb/docker.hlb"
}
```

2. Create a `module.hlb` in the new directory. This is the entrypoint to your module. You can create more HLB modules or subdirectories to organize your implementation, but the interface end users invoke will be from `module.hlb`.

3. Write doxygen-style docs for the exported functions, we currently only support `@param` and `@return` but the typical structure is:

```hlb
# A high-level description of what this function does. Lines should respect the
# 80 character limit per line, and should be written in full sentences,
# complete with punctuation.
#
# Separated by a new-line, additional context to the usage of this function can
# be followed here.
#
# @param foo A description of what the function expects from `foo`.
# @return A description of what the function returns.
fs myFunction(string foo) {
	# ...
}
```

3. Create a `README.md` in the new directory with examples of an end-user importing and using the module.

## Guidelines

- Avoid redundant naming between the module name and the function name. For example, the module `docker` exports the function `build` so users can call `docker.build` instead of `docker.dockerBuild`.
- Be agnostic to the input the user wants to provide by plumbing `fs` as a function argument. For example, don't call `local` and instead allow the user to provide a `fs input` so they can decide whether it comes from their local system, from DockerHub, or from a git remote.
- Don't write monolithic functions, try to keep each function to have one purpose. The more granular the functions, the more concurrent the graph can be executed.
- Provide helper options for typical usage. For example, if using `local` with a specific `includePattern` is common, provide a helper function with type `option::local`.
