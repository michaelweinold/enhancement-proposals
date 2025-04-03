# BEP-0005 Brightway Dependency Management Strategy

| | |
| - | - |
| Number | 5 |
| Title | Brightway Dependency Management Strategy |
| Status | **Withdrawn** |
| Type | Guidelines |
| Proposed By | [Michael Weinold](mailto:dev@weinold.ch) |
| Created | 2023-08-01 |
| Last updated | 2025-04-13 |
| Version | 3 |

This proposal has been withdrawn. This is because Brightway has many, many, many dependencies[1] and only a very limited number of contributors with the expertise or interest in platform-specific dependency management. Development is (unpythonically, for historical and pragmatic reasons) spread out across multiple repositories (see stale `BEP0002`) and running Brightway in a WebAssembly environment is not a priority for any contributors/maintainers. While https://webapp.brightway.dev and https://live.brightway.dev are a testament to the enthusiasm of the authors, such applications will remain a novelty for the forseeable future.

[1] A list of `brightway25==1.1.0` dependencies:

1. annotated-types
2. asteval
3. astunparse
4. blinker
5. brightway25
6. brotli
7. bw2analyzer
8. bw2calc
9. bw2data
10. bw2io
11. bw2parameters
12. bw_migrations
13. bw_processing
14. bw_simapro_csv
15. certifi
16. charset-normalizer
17. contourpy
18. cycler
19. deepdiff
20. deprecated
21. ecoinvent_interface
22. et-xmlfile
23. flexcache
24. flexparser
25. fonttools
26. fsspec
27. ftfy
28. idna
29. inflate64
30. kiwisolver
31. loguru
32. lxml
33. matrix_utils
34. matplotlib
35. morefs
36. mrio_common_metadata
37. multifunctional
38. multivolumefile
39. numpy
40. openpyxl
41. ordered-set
42. packaging
43. pandas
44. peewee
45. pillow
46. pint
47. platformdirs
48. psutil
49. py7zr
50. pybcj
51. pycryptodomex
52. pydantic
53. pydantic-core
54. pydantic-settings
55. pyecospold
56. pyparsing
57. pyppmd
58. python-dateutil
59. python-dotenv
60. pytz
61. pyxlsb
62. pyzstd
63. randonneur
64. randonneur_data
65. rapidfuzz
66. rdflib
67. requests
68. scipy
69. six
70. snowflake-id
71. SPARQLWrapper
72. stats_arrays
73. structlog
74. tabulate
75. texttable
76. tqdm
77. typing_extensions
78. typing-inspection
79. tzdata
80. urllib3
81. voluptuous
82. wcwidth
83. wheel
84. wrapt
85. xlrd
86. xlsxwriter

## Abstract

New dependencies for Brightway should be either: \
Platform independent (with support for the three major plaforms [`x86_64`, `aarch64`, `wasm32`, as defined in  PEP11](https://peps.python.org/pep-0011/)), or... \
Fail gracefully only on first call (not on package import), according to the _"lazy import"_ logic defined in [PEP690](https://peps.python.org/pep-0690/). \
This will ensure that, without additional maintainer effort, Brightway will remain compatible with WebAssembly and Apple Silicon.

## Motivation

As the Python/WebAssembly ecosystem (eg. [Pyodide](https://pyodide.org/en/stable/), [JupyterLite](https://jupyterlite.readthedocs.io/en/stable/), [WASM as a 1st class Python platform](https://discuss.python.org/t/make-wasm-a-1st-class-platform-in-the-python-ecosystem/21798), etc.) grows, "Python in the Browser" will likely become ubiquitous. This will offer new opportunities for introducing new users to Brightway, or quickly setting up "beginner-level" classes in a JupyterHub-lite environment. Time/Resource-intensive maintenance of the usually required JupyterHub servers can be replaced by a simple web server hosting a static website. Platform-dependent installation issues will be a thing of the past. Similarly, the adoption of Apple Silicon will likely increase, as Apple has announced that they will transition their entire product line to Apple Silicon. Some Brightway dependencies are not yet compatible with either WebAssembly or Apple Silicon. This will require that Brightway adhere to well-established dependency best-practices of the Python ecosystem. 

## Proposal

For any new dependency, the following determination shall be made:

1. Is the dependency compatible with the three major platforms [`x86_64`, `aarch64`, `wasm32`, as defined in  PEP11](https://peps.python.org/pep-0011/)?
2. Is a platform-specific version of the dependency available for the platform(s) not supported in 1.? If so, the dependency shall be introduced as a platform-specific dependency, with the appropriate `setup.py` logic.
2. If neither 1. or 2. apply, the dependency shall fail gracefully only on first call of the related function, according to the _"lazy import"_ logic defined in [PEP690](https://peps.python.org/pep-0690/).

This will ensure that Brightway packages can still be imported, even if the dependency is not available for a specific platform.

### Rationale

Brightway will adopt a weaker version of PEP690, where lazy imports are required only for dependencies that are not compatible with all target platforms. Ensuring that all future dependencies adhere to BEP005 will allow developers to maintain compatibility with WebAssembly and Apple Silicon without additional effort.

### Pros and Cons

Pros: Brightway will remain compatible with WebAssembly and Apple Silicon without additional maintainer effort.

Cons: None

### Alternatives

Not introduce a dependency management strategy.

### Open Issues

N/A

### Non-Goals

N/A

### Test plan and results

N/A

## Discussion

- https://github.com/brightway-lca/brightway-live/issues/7
- https://github.com/brightway-lca/brightway-live/discussions/21

## Previous Versions

- Version 1 (unpublished, part of [PR#29](https://github.com/brightway-lca/enhancement-proposals/pull/29))
- Version 2 (unpublished)
- Version 3 (this version, withdrawn)