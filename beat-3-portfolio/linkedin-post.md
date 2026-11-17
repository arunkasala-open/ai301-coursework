This fall, I made my first open-source contribution to qim3d, a free tool scientists use to analyze 3D images, such as material scans. I found that some of the project's test files were not organized properly, so I split a large test file into two smaller files, moved four tests into two new files beside the related code, and removed an empty folder.

These tests are useful because they help developers check whether the software is still working properly after making changes. I did not want my file organization changes to accidentally remove or affect any existing tests. So, before making the changes, I created a list of all the tests and compared it with the list after completing the work. The comparison showed that all 43 tests were still present, with exactly the same names as before. I also ran the project's test suite and confirmed that all 318 tests were passing.

At the moment, I have submitted my contribution and it is waiting for review from the project maintainers. I learned that contributing to a project is not always about adding new features or writing a lot of code. Sometimes, improving the organization of existing code is also valuable. Most importantly, I learned to follow the project's existing guidelines, make changes carefully, and provide evidence that my work has not affected the existing functionality.

https://github.com/qim-center/qim3d/pull/298
