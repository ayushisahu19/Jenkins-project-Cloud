# Jenkins Assignment Evidence

Student alias (Git author): ayushisahu19
Class code: 001 | Challenge code: 00744
Job: festpass-ci (Pipeline script from SCM, git://127.0.0.1:9418/festpass-java, branch */main, Jenkinsfile, agent student-laptop)

## Red build

- Build: festpass-ci #2
- Cause: Started by an SCM change (no manual click)
- Commit: 748fb416e468716eb1a94a64b28b0c131941b881 ("Personalize my help introduction")
- Result: FAILURE, 28 tests, 3 failures
- Skipped stages: Build JAR, Smoke test JAR, Save artifact. No JAR was published.
- Evidence CSV: evidence/red/submission-evidence.csv (JarSha256 = NOT_PUBLISHED)
- Failed assertion: cancellationFreesSeat, `java.lang.IllegalArgumentException: SOLD OUT. Even the developer needs a ticket.`

## Diagnosis

`BookingService.cancel` checked that the student had a booking but never removed them from the event's roster. Cancelling therefore did not free the seat, so booking the next student into the freed seat was rejected as sold out, and cancellations were also lost after a reload.

## Fix

Commit 93e9409 added one line to `cancel`: `event.students.remove(studentId);`. No tests, build helpers, evidence code or pipeline stages were changed. The only Jenkinsfile change is the `pollSCM('* * * * *')` trigger.

## Green build

- Build: festpass-ci #5
- Cause: Started by an SCM change (no manual click)
- Commit: 1ce30833f65542a54636e18835164e167de2155f ("Remove stray comment from FestPass.java")
- Result: SUCCESS, 28 tests, 0 failures, 0 errors, 0 skipped
- Artifacts: festpass.jar, build-info.txt, smoke-test.txt, submission-evidence.csv
- Evidence CSV: evidence/green/submission-evidence.csv
- JAR: evidence/green/festpass.jar (SHA-256 360873dd7ec4c4c2b6b53255d3bbb5e0480c1569d17758e2e205de485aa0453f, matches the CSV)

`java -jar festpass.jar help` prints: `FestPass - campus event registration (by ayushisahu19)`

## Note on my commits

In commit 748fb41 I first added my alias as a comment on line 1 instead of in the help text by mistake. Commit f3b3866 put it in the real help text, and commit 1ce3083 removed the leftover comment.

## Screenshots

- evidence/screenshots/build2-red.png
- evidence/screenshots/build2-failed-test.png
- evidence/screenshots/build5-green.png
