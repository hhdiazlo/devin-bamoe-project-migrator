# Spring Boot target for the BAMOE migrator (preview)

Adds a Spring Boot target alongside Quarkus (the default). Quarkus behaviour is
unchanged: the validator gives identical output and exit codes before and after
this change on 15 Quarkus projects (see "Quarkus parity" below).

## What it adds

- `AGENTS.md`: runtime table (Quarkus or Spring Boot), rule 12 split by runtime,
  new rule 12b (the registered handler name must equal the BPMN `drools:taskName`)
- Spring paths in the migrate, project-structure and BPMN skills, and in plan Sub-Task 6
- `templates/spring-pom.xml`, `spring-application.properties`,
  `SpringBootMain.java.tmpl`, `SpringWorkItemHandlerConfig.java.tmpl`
- `tools/validate-migration.sh`: Spring mode (auto-detected from the pom, or
  `--spring` / `--quarkus`)

## What it was tested against

BAMOE 9.4.2-ibm-0002, Spring Boot 3.5.0, JDK 21, H2, Spring MVC, one process,
one custom work item handler. Every Spring rule is tagged `Confirmed-by-build`
or `Untested`.

## What is NOT tested

- Timer in migrated output, error and abort paths, OIDC / Spring Security
- BAMOE 9.5.x, Spring Boot 4, JDK 17
- PostgreSQL migration of a real project
- Whether Devin follows the new wording: no agent run has been done yet

## Known gaps (to be added after the first test run)

- Handler method signatures for 9.4.2 are not in the skills
- The nested-generics limit (a `List<Map<String,Object>>` model field fails
  proto generation) is not documented
- `kogito.dataindex.http.url` (process-svg add-on) is not documented
- `bamoe-deploy-verify` and `bamoe-migrate-security` have no Spring guidance;
  plan Sub-Tasks 2, 8, 9 and 10 are still Quarkus-only

## Quarkus parity

Original and patched validator were run on the 2 projects in `v9-projects/` and
the 13 sample `v9-app` directories that have a `v8-app` (tutorial-01 to -14 minus
-13, which has no `v9-app`). Output and exit code were identical for all 15.
The tutorial-03 and tutorial-12 `v9-app` answers are removed from this branch
for the test run, so those two cannot be reproduced from here.

## Prompt to use in Devin

    Migrate my project to BAMOE 9.4.2 on Spring Boot 3.5

Set `BAMOE_MAVEN_REPO_URL` as a Devin secret to enable build verification.
Do not read or check out any `evidence/*` branch.
