<!--
SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
-->

# Frequently Asked Questions

## What is the relationship between the Beman Project and the Boost Foundation?

While both Beman Project and Boost strive to provide high-quality C++ libraries to the community, the Beman Project is specifically focused on fostering works that can be adopted by the standard library.
Note that we expect some Boost libraries will become part of Beman over time.

## Do I have to write a WG21 C++ Standards Committee Paper before I can contribute to the Beman Project?

No!
The Beman Project wants to work with you, with experience, feedback, and expertise.
Obviously, a paper must be written before becoming accepted as part of the standard, but that paper can be written in tandem with your library development.

In fact, there are plenty of papers already written that need an implementation before they can progress in through the standards track.
You can contribute to the Beman project by finding a paper that interests you and working on its library.

Please reach out to the Beman Project Leads to help get your specific project started.

## I have a library that isn't appropriate for the C++ standard. Can I contribute it to the Beman Project?

The Beman Project is dedicated to getting the highest quality libraries through the rigorous effort of standardization.
Libraries not falling in this category are out of scope.

## I have a personal/work project but I have no interest in it becoming a standard library. Does the Beman Project have anything for me?

Yes!
In our effort to create the highest-quality C++ libraries, we've established a set of templates, tools, and guidelines that are broadly applicable even for projects not destined to be standardized. For example, our [Exemplar Project](https://github.com/bemanproject/exemplar) demonstrates a useful structure for organizing libraries

## Will libraries stay in Beman forever?

No!
The current concept is that libraries will be gradually deprecated as official standard library implementations roll out. We expect this to correspond to two standards cycles. So a library accepted into C++26 would be removed by C++32. Users can, of course, still depend on the original repo for the implementation if they don't want to change to `~std::~` for some reason.

## Is there a review process like Boost?

No!
Inclusion in Beman doesn't depend on passing a formal review.
That said, authors can ask the community for reviews at any time!
Also, we expect libraries to evolve and using the code review facilities allows authors to get other eyes on updates as proposals evolve.

## Is there a minimum C++ version required for all Beman Projects to support?

No!
The Beman Project does not define a minimum C++ version for all Beman Projects to support,
nor does it define a lower boundary for the minimum C++ version to support.
The C++ version support range is decided by the library author and differs across projects.
Note that given the nature of the Beman Project,
it is likely that you'll need to use recent releases of the C++ standard to play around with the projects.
