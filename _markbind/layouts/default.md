<head-bottom>
  <link rel="stylesheet" href="{{baseUrl}}/stylesheets/main.css">
</head-bottom>

<header sticky>
  <navbar type="dark">
    <a slot="brand" href="{{baseUrl}}/index.html" title="Home" class="navbar-brand">CS3203</a>
    <!-- <li><a href="{{baseUrl}}/contents/team.html" class="nav-link">Team Formation</a></li>
    <dropdown header="Tools" class="nav-link">
      <li><a href="{{baseUrl}}/contents/tools/introduction.html" class="dropdown-item">Introduction</a></li>
      <li><a href="{{baseUrl}}/contents/tools/downloads.html" class="dropdown-item">Downloads</a></li>
      <li><a href="{{baseUrl}}/contents/tools/windows-spa.html" class="dropdown-item">Windows Startup SPA Solution<a></li>
      <li><a href="{{baseUrl}}/contents/topic3a.html" class="dropdown-item">Cross-platform Startup SPA Solution</a></li>
      <li><a href="{{baseUrl}}/contents/topic3a.html" class="dropdown-item">Autotester Integration and Testing</a></li>
      <li><a href="{{baseUrl}}/contents/topic3a.html" class="dropdown-item">Version Control System and Code Repository</a></li>
      <li><a href="{{baseUrl}}/contents/topic3a.html" class="dropdown-item">Project Management</a></li>
      <li><a href="{{baseUrl}}/contents/topic3a.html" class="dropdown-item">Continuous Integration</a></li>
    </dropdown> -->
    <li slot="right">
      <form class="navbar-form">
        <searchbar :data="searchData" placeholder="Search" :on-hit="searchCallback" menu-align-right></searchbar>
      </form>
    </li>
  </navbar>
</header>

<div id="flex-body">
  <nav id="site-nav">
    <div class="site-nav-top">
      <div class="fw-bold mb-2" style="font-size: 1.25rem;">Content</div>
    </div>
    <div class="nav-component slim-scroll">
      <site-nav>
* [Home :house:]({{ baseUrl }}/index.html)
* [Update History]({{baseUrl}}/contents/update-history.html)
* [Team Formation]({{baseUrl}}/contents/team-formation.html)
* Tools :expanded:
  * [Introduction]({{baseUrl}}/contents/tools/introduction.html)
  * [Downloads]({{baseUrl}}/contents/tools/downloads.html)
  * [Windows Startup SPA Solution]({{baseUrl}}/contents/tools/windows-spa.html)
  * [Cross-platform Startup SPA Solution]({{baseUrl}}/contents/tools/cross-platform-spa.html)
  * [Autotester Integration and Testing]({{baseUrl}}/contents/tools/autotester-testing.html)
  * [Version Control System and Code Repository]({{baseUrl}}/contents/tools/version-control-repository.html)
  * [Project Management]({{baseUrl}}/contents/tools/project-management.html)
  * [Continuous Integration]({{baseUrl}}/contents/tools/continuous-integration.html)
* SPA Requirements - Basic SPA :expanded:
  * [Motivation]({{baseUrl}}/contents/basic-spa-requirements/motivation.html)
  * [SIMPLE Programming Language]({{baseUrl}}/contents/basic-spa-requirements/simple-programming.html)
  * [Abstract Syntax Tree]({{baseUrl}}/contents/basic-spa-requirements/abstract-syntax-tree.html)
  * [Design Entities]({{baseUrl}}/contents/basic-spa-requirements/design-entities.html)
  * [Design Abstractions]({{baseUrl}}/contents/basic-spa-requirements/design-abstractions.html)
  * Program Query Language (PQL) :expanded:
    * [Introduction]({{baseUrl}}/contents/basic-spa-requirements/program-query-language/introduction.html)
    * Further PQL Discussion :expanded:
      * [Such-That Clauses]({{baseUrl}}/contents/basic-spa-requirements/program-query-language/such-that-clause.html)
      * [Assign Pattern Clauses]({{baseUrl}}/contents/basic-spa-requirements/program-query-language/assign-pattern-clause.html)
    * [Example queries]({{baseUrl}}/contents/basic-spa-requirements/program-query-language/example-queries.html)
  * [Intended Behaviour & Format of Results]({{baseUrl}}/contents/basic-spa-requirements/intended-behaviour-format-results.html)
  * [FAQ]({{baseUrl}}/contents/basic-spa-requirements/faq.html)
* SPA Requirements - Advanced SPA :expanded:
  * [Control Flow Graph]({{baseUrl}}/contents/advanced-spa-requirements/cfg.html)
  * [Design Abstractions]({{baseUrl}}/contents/advanced-spa-requirements/design-abstractions.html)
  * PQL :expanded:
    * [Introduction]({{baseUrl}}/contents/advanced-spa-requirements/pql.html)
    * [Select Clauses]({{baseUrl}}/contents/advanced-spa-requirements/PQL/select-clauses.html)
    * [Such-That Clauses]({{baseUrl}}/contents/advanced-spa-requirements/PQL/such-that-clauses.html)
    * [Pattern Clauses]({{baseUrl}}/contents/advanced-spa-requirements/PQL/pattern-clauses.html)
    * [With Clauses]({{baseUrl}}/contents/advanced-spa-requirements/PQL/with-clauses.html)
    * [Multi Clauses]({{baseUrl}}/contents/advanced-spa-requirements/PQL/multi-clauses.html)
    * [Example Queries]({{baseUrl}}/contents/advanced-spa-requirements/PQL/example-queries.html)
  * [Format of Results]({{baseUrl}}/contents/advanced-spa-requirements/format-res.html)
* Project Requirements - Milestone 1 :expanded:
  * [Scope]({{baseUrl}}/contents/project-requirements-1/scope.html)
  * [Administrative Matters]({{baseUrl}}/contents/project-requirements-1/admin.html)
  * [Tips]({{baseUrl}}/contents/project-requirements-1/tips.html)
* Project Requirements - Milestone 2 :expanded:
  * [Scope]({{baseUrl}}/contents/project-requirements-2/scope.html)
  * [Administrative Matters]({{baseUrl}}/contents/project-requirements-2/admin.html)
  * [Tips]({{baseUrl}}/contents/project-requirements-2/tips.html)
* Project Requirements - Milestone 3 :expanded:
  * [Scope]({{baseUrl}}/contents/project-requirements-3/scope.html)
  * [Administrative Matters]({{baseUrl}}/contents/project-requirements-3/admin.html)
* Project Requirements - Guidelines :expanded:
  * Code Submission Guidelines :expanded:
    * [Introduction]({{baseUrl}}/contents/project-requirement-guidelines/csg.html)
    * [Submission Format Checker]({{baseUrl}}/contents/project-requirement-guidelines/sub-format-check.html)
  * [Report Submission Guidelines]({{baseUrl}}/contents/project-requirement-guidelines/rsg.html)
  * [Code Demonstration Guidelines]({{baseUrl}}/contents/project-requirement-guidelines/cdg.html)
  * [Oral Presentation Guidelines]({{baseUrl}}/contents/project-requirement-guidelines/opg.html)
  * [Final Presentation Guidelines]({{baseUrl}}/contents/project-requirement-guidelines/fp.html)
  * [Grading Guidelines]({{baseUrl}}/contents/project-requirement-guidelines/gg.html)
* [Reference Books]({{baseUrl}}/contents/reference.html)
      </site-nav>
    </div>
  </nav>
  <div id="content-wrapper">
    {{ content }}
  </div>
  <nav id="page-nav">
    <div class="nav-component slim-scroll">
      <page-nav />
    </div>
  </nav>
</div>

<footer>
  <!-- Support MarkBind by including a link to us on your landing page! -->
  <div class="text-center">
    <small>[Generated by {{MarkBind}}]</small>
  </div>
</footer>
