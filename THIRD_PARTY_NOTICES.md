# Third-Party Notices

This project bundles a single third-party JavaScript library — **Mermaid**
(version `11.14.0`) — both as the standalone reference file
`vendor/mermaid.min.js` and inlined into `mermaid-sandbox.html`. Mermaid
is itself a webpack bundle that includes a number of transitive
libraries; their attributions are reproduced below in addition to
Mermaid's own.

The full text of the **MIT License** (under which most listed components
are distributed) is in `vendor/LICENSE-mermaid.txt`. The full text of the
**Apache License 2.0** is included at the bottom of this file.

If you redistribute this software, you must retain this file alongside
the binaries.

## Bundled in `mermaid.min.js` (and the inlined copy in `mermaid-sandbox.html`)

| Component                  | Version*  | License                  | Project URL                                                       |
|----------------------------|-----------|--------------------------|-------------------------------------------------------------------|
| mermaid                    | 11.14.0   | MIT                      | https://github.com/mermaid-js/mermaid                             |
| @mermaid-js/parser         | bundled   | MIT                      | https://github.com/mermaid-js/mermaid                             |
| @braintree/sanitize-url    | bundled   | MIT                      | https://github.com/braintree/sanitize-url                         |
| @iconify/utils             | bundled   | MIT                      | https://github.com/iconify/iconify                                |
| @upsetjs/venn.js           | bundled   | MIT                      | https://github.com/upsetjs/venn.js                                |
| chevrotain                 | bundled   | Apache-2.0               | https://github.com/chevrotain/chevrotain                          |
| cytoscape                  | bundled   | MIT                      | https://github.com/cytoscape/cytoscape.js                         |
| cytoscape-cose-bilkent     | bundled   | MIT                      | https://github.com/cytoscape/cytoscape.js-cose-bilkent            |
| cytoscape-fcose            | bundled   | MIT                      | https://github.com/iVis-at-Bilkent/cytoscape.js-fcose             |
| d3 (and d3-* sub-packages) | bundled   | BSD-3-Clause / ISC       | https://github.com/d3/d3                                          |
| d3-sankey                  | bundled   | BSD-3-Clause             | https://github.com/d3/d3-sankey                                   |
| dagre-d3-es                | bundled   | MIT                      | https://github.com/CodSpeedHQ/dagre-d3-es                         |
| dayjs                      | bundled   | MIT                      | https://github.com/iamkun/dayjs                                   |
| dompurify                  | bundled   | Apache-2.0 OR MPL-2.0    | https://github.com/cure53/DOMPurify                               |
| katex                      | bundled   | MIT                      | https://github.com/KaTeX/KaTeX                                    |
| khroma                     | bundled   | MIT                      | https://github.com/fabiospampinato/khroma                         |
| langium                    | bundled   | MIT                      | https://github.com/eclipse-langium/langium                        |
| lodash-es                  | bundled   | MIT                      | https://github.com/lodash/lodash                                  |
| marked                     | bundled   | MIT                      | https://github.com/markedjs/marked                                |
| roughjs                    | bundled   | MIT                      | https://github.com/rough-stuff/rough                              |
| stylis                     | bundled   | MIT                      | https://github.com/thysultan/stylis                               |
| ts-dedent                  | bundled   | MIT                      | https://github.com/tamino-martinius/node-ts-dedent                |
| uuid                       | bundled   | MIT                      | https://github.com/uuidjs/uuid                                    |

\* "bundled" means the version is whichever Mermaid 11.14.0 pinned at
release time. Mermaid bundles these transitively — they are not
separately versioned in this repository.

For dual-licensed components (e.g. dompurify under "Apache-2.0 OR
MPL-2.0"), this project uses them under the **Apache-2.0** option.

## Apache-2.0 components

`chevrotain` and `dompurify` are distributed under the Apache License
Version 2.0. The license text is reproduced below per its
redistribution requirements. No NOTICE file content from the upstream
projects is reproduced here because, at the version bundled by Mermaid
11.14.0, neither project ships a non-empty NOTICE file. If a future
re-vendoring changes that, copy the upstream NOTICE content into this
section.

## BSD-3-Clause components

`d3` and `d3-sankey` are distributed under the BSD 3-Clause License.
Copyright Mike Bostock. The full BSD-3-Clause text is short and
identical across these projects:

```
Copyright (c) 2010-2024 Mike Bostock

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of the author nor the names of contributors may be used to
  endorse or promote products derived from this software without specific
  prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
```

## Apache License Version 2.0

```
                                 Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   1. Definitions.

      "License" shall mean the terms and conditions for use, reproduction,
      and distribution as defined by Sections 1 through 9 of this document.

      "Licensor" shall mean the copyright owner or entity authorized by
      the copyright owner that is granting the License.

      "Legal Entity" shall mean the union of the acting entity and all
      other entities that control, are controlled by, or are under common
      control with that entity. For the purposes of this definition,
      "control" means (i) the power, direct or indirect, to cause the
      direction or management of such entity, whether by contract or
      otherwise, or (ii) ownership of fifty percent (50%) or more of the
      outstanding shares, or (iii) beneficial ownership of such entity.

      "You" (or "Your") shall mean an individual or Legal Entity
      exercising permissions granted by this License.

      "Source" form shall mean the preferred form for making modifications,
      including but not limited to software source code, documentation
      source, and configuration files.

      "Object" form shall mean any form resulting from mechanical
      transformation or translation of a Source form, including but
      not limited to compiled object code, generated documentation,
      and conversions to other media types.

      "Work" shall mean the work of authorship, whether in Source or
      Object form, made available under the License, as indicated by a
      copyright notice that is included in or attached to the work
      (an example is provided in the Appendix below).

      "Derivative Works" shall mean any work, whether in Source or Object
      form, that is based on (or derived from) the Work and for which the
      editorial revisions, annotations, elaborations, or other modifications
      represent, as a whole, an original work of authorship. For the purposes
      of this License, Derivative Works shall not include works that remain
      separable from, or merely link (or bind by name) to the interfaces of,
      the Work and Derivative Works thereof.

      "Contribution" shall mean any work of authorship, including
      the original version of the Work and any modifications or additions
      to that Work or Derivative Works thereof, that is intentionally
      submitted to Licensor for inclusion in the Work by the copyright owner
      or by an individual or Legal Entity authorized to submit on behalf of
      the copyright owner. For the purposes of this definition, "submitted"
      means any form of electronic, verbal, or written communication sent
      to the Licensor or its representatives, including but not limited to
      communication on electronic mailing lists, source code control systems,
      and issue tracking systems that are managed by, or on behalf of, the
      Licensor for the purpose of discussing and improving the Work, but
      excluding communication that is conspicuously marked or otherwise
      designated in writing by the copyright owner as "Not a Contribution."

      "Contributor" shall mean Licensor and any individual or Legal Entity
      on behalf of whom a Contribution has been received by Licensor and
      subsequently incorporated within the Work.

   2. Grant of Copyright License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare Derivative Works of,
      publicly display, publicly perform, sublicense, and distribute the
      Work and such Derivative Works in Source or Object form.

   3. Grant of Patent License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      (except as stated in this section) patent license to make, have made,
      use, offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by such Contributor that are necessarily infringed by their
      Contribution(s) alone or by combination of their Contribution(s)
      with the Work to which such Contribution(s) was submitted. If You
      institute patent litigation against any entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that the Work
      or a Contribution incorporated within the Work constitutes direct
      or contributory patent infringement, then any patent licenses
      granted to You under this License for that Work shall terminate
      as of the date such litigation is filed.

   4. Redistribution. You may reproduce and distribute copies of the
      Work or Derivative Works thereof in any medium, with or without
      modifications, and in Source or Object form, provided that You
      meet the following conditions:

      (a) You must give any other recipients of the Work or
          Derivative Works a copy of this License; and

      (b) You must cause any modified files to carry prominent notices
          stating that You changed the files; and

      (c) You must retain, in the Source form of any Derivative Works
          that You distribute, all copyright, patent, trademark, and
          attribution notices from the Source form of the Work,
          excluding those notices that do not pertain to any part of
          the Derivative Works; and

      (d) If the Work includes a "NOTICE" text file as part of its
          distribution, then any Derivative Works that You distribute must
          include a readable copy of the attribution notices contained
          within such NOTICE file, excluding those notices that do not
          pertain to any part of the Derivative Works, in at least one
          of the following places: within a NOTICE text file distributed
          as part of the Derivative Works; within the Source form or
          documentation, if provided along with the Derivative Works; or,
          within a display generated by the Derivative Works, if and
          wherever such third-party notices normally appear. The contents
          of the NOTICE file are for informational purposes only and
          do not modify the License. You may add Your own attribution
          notices within Derivative Works that You distribute, alongside
          or as an addendum to the NOTICE text from the Work, provided
          that such additional attribution notices cannot be construed
          as modifying the License.

      You may add Your own copyright statement to Your modifications and
      may provide additional or different license terms and conditions
      for use, reproduction, or distribution of Your modifications, or
      for any such Derivative Works as a whole, provided Your use,
      reproduction, and distribution of the Work otherwise complies with
      the conditions stated in this License.

   5. Submission of Contributions. Unless You explicitly state otherwise,
      any Contribution intentionally submitted for inclusion in the Work
      by You to the Licensor shall be under the terms and conditions of
      this License, without any additional terms or conditions.
      Notwithstanding the above, nothing herein shall supersede or modify
      the terms of any separate license agreement you may have executed
      with Licensor regarding such Contributions.

   6. Trademarks. This License does not grant permission to use the trade
      names, trademarks, service marks, or product names of the Licensor,
      except as required for describing the origin of the Work and
      reproducing the content of the NOTICE file.

   7. Disclaimer of Warranty. Unless required by applicable law or
      agreed to in writing, Licensor provides the Work (and each
      Contributor provides its Contributions) on an "AS IS" BASIS,
      WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
      implied, including, without limitation, any warranties or conditions
      of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
      PARTICULAR PURPOSE. You are solely responsible for determining the
      appropriateness of using or redistributing the Work and assume any
      risks associated with Your exercise of permissions under this License.

   8. Limitation of Liability. In no event and under no legal theory,
      whether in tort (including negligence), contract, or otherwise,
      unless required by applicable law (such as deliberate and grossly
      negligent acts) or agreed to in writing, shall any Contributor be
      liable to You for damages, including any direct, indirect, special,
      incidental, or consequential damages of any character arising as a
      result of this License or out of the use or inability to use the
      Work (including but not limited to damages for loss of goodwill,
      work stoppage, computer failure or malfunction, or any and all
      other commercial damages or losses), even if such Contributor
      has been advised of the possibility of such damages.

   9. Accepting Warranty or Support. While redistributing the Work or
      Derivative Works thereof, You may choose to offer, and charge a
      fee for, acceptance of support, warranty, indemnity, or other
      liability obligations and/or rights consistent with this License.
      However, in accepting such obligations, You may act only on Your
      own behalf and on Your sole responsibility, not on behalf of any
      other Contributor, and only if You agree to indemnify, defend,
      and hold each Contributor harmless for any liability incurred by,
      or claims asserted against, such Contributor by reason of your
      accepting any such warranty or support.

   END OF TERMS AND CONDITIONS
```

## Trademarks

"Mermaid" is a project name of the Mermaid project. "Obsidian" is a
trademark of Dynalist Inc. "GitHub" is a trademark of GitHub, Inc.
These names appear in this project's documentation only as nominative
references to describe what is bundled or what is interoperable; no
endorsement is implied and no trademark rights are claimed.
