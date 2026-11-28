# Specialized Backend Challenge Learning Track / 后端编程挑战专项学习单元

## 1. Purpose / 学习定位

A reusable preparation track for backend, platform, SRE and DevSecOps coding assessments that combine constrained Python, source handoff, HTTP and standards-based authentication.

Entry level: arrays, hash maps/sets, basic Python functions and conditions; recursion, standard I/O and cryptographic APIs may be unfamiliar. This is a temporary bridge, not a replacement for the agreed DSA roadmap. If the learner has just finished Arrays & Hashing, return to the next agreed roadmap topic (typically Two Pointers) after this track. Do not infer independent mastery from roadmap completion alone.

Follow [SKILL.md](SKILL.md), its worked exemplars and [session template](templates/PROBLEM_SESSION_TEMPLATE.md). Use Chinese explanations with English technical vocabulary by default. Each lesson is a separate teaching turn or series of turns, not a single answer dump.

## 2. Learning boundaries / 独立完成与保密

- Check the assessment's own permitted language, runtime, syntax, libraries, outside assistance and submission rules locally. They override generic study examples.
- For an active assessment requiring independent work, teach transferable concepts with unrelated exercises, official documentation and local mock systems. Do not produce submission-ready solutions, tailored pseudocode, authentication scripts or real challenge requests; do not publish or submit on the learner's behalf.
- Do not store proprietary assessment statements, exact test parameters, candidate-specific URLs, tokens, emails, real credentials, solution files or private progress in this public skill. Use fictional placeholders and public standards only.
- No brute force, probing or repeated unauthorized requests. A local simulation is not evidence of external acceptance.

## 3. Roadmap / 学习大纲

Suggested study time: 405 minutes (6h 45m), excluding remediation and the real independent assessment. These are planning estimates, not a deadline or guarantee.

| Lesson | Topic | Minutes | Exit evidence |
| --- | --- | ---: | --- |
| 01 | Python standard I/O and strings | 30 | Parse a fictional line-oriented input stream and produce exact output |
| 02 | Functions and functional tools | 45 | Explain and independently use map/filter/sum and named functions |
| 03 | Recursion and call stack | 60 | Trace base case, recursive case, return values and depth |
| 04 | Constrained coding and testing | 60 | Independently solve an unrelated exercise under explicit restrictions |
| 05 | GitHub Secret Gist | 15 | Understand single-file handoff and link visibility |
| 06 | HTTP, JSON and Basic Authentication | 30 | Send and inspect a request to a local mock server |
| 07 | Bytes, hashing and HMAC | 45 | Distinguish encoding, hashing, HMAC and encryption |
| 08 | HOTP/TOTP and RFC reading | 60 | Explain time counters, truncation and verify public test vectors |
| 09 | Local integration and troubleshooting | 60 | Independently demonstrate the mock workflow and diagnose failures |

### Phase A — Python & algorithms

#### Lesson 01 — Standard Input / Output

- stdin, stdout, stderr; input(), print(), sys.stdin.readline(); line-based vs token-based parsing; bounded reads vs waiting for EOF.
- str.split(), int(), len(), whitespace, newline handling, declared count vs actual count.
- Distinguish parsing, validation, processing and output formatting. Learn when to buffer results.
- Practice: a fictional inventory input containing a known number of text records; preserve record boundaries and print normalized labels after reading.
- Gate: explain why reading until EOF is wrong for a stream that remains open; trace parsing of a line with extra whitespace; write a small independent parser and compare exact output.

#### Lesson 02 — Functions & Functional Programming

- def, parameters, return vs print, local scope, function objects, lambda, iterables and iterators.
- map(), filter(), sum(), list(), lazy evaluation and consumption; explain predicate vs transform vs aggregation with small unrelated examples.
- Distinguish Python syntax restrictions from algorithmic iteration; built-ins are not automatically permitted by every assessment.
- Practice: normalize fictional product labels and separately compute a total of string lengths.
- Gate: predict iterator output and explain when data is consumed; write named-function and lambda variants where permitted.

#### Lesson 03 — Recursion & Call Stack

- Base case, recursive case, progress toward termination, call stack, return/unwind, recursion depth and space complexity.
- Start from a simple loop as a conceptual baseline only for unrestricted practice, then derive recursion. If the live assessment bans loops, never present a banned solution as compliant.
- Practice: factorial of a small non-negative number and recursive string reversal; trace stack frames, including empty/base input.
- Gate: draw a trace, explain why missing progress fails, implement a different small recursion independently. Discuss Python recursion limits and lack of tail-call optimization; do not change limits without justification.

#### Lesson 04 — Restricted Coding & Verification

- Read constraints before choosing a method; distinguish syntax bans (loops, comprehensions, globals) from permitted built-ins.
- Design a single-file entry point, pure helpers, deterministic state, and exact I/O.
- Practice: an unrelated label-processing task under a fictional no-explicit-loop restriction, with its own requirements and tests; do not reproduce the active challenge.
- Test categories: minimum and maximum allowed input, whitespace, empty values where allowed, invalid record metadata, multiple records, output format, termination without EOF, and runtime/libraries.
- Gate: learner writes their own practice implementation; coach uses H1–H3 hints and distinguishes manual trace from actual execution. Check the applicable syntax rules and derive time/space from submitted code.

### Phase B — Engineering & API fundamentals

#### Lesson 05 — GitHub Secret Gist & Source Handoff

- Gist vs repository; secret vs public Gist; link-based visibility is not strong access control.
- Learn file naming, source-only submission, correct extension and removing secrets/debug output.
- Practice using a harmless invented file, locally or in a learner-created secret Gist if explicitly requested. Never publish real assessment material in the public Skills repository.
- Gate: explain what a secret Gist does and does not protect; identify the correct link and verify file count without exposing confidential content.

#### Lesson 06 — HTTP POST, JSON & Basic Authentication

- Request line, headers, body, response status; JSON serialization vs Python dict; Content-Type vs Accept.
- Basic Auth: username/password encoding, Base64 is not encryption, HTTPS matters. Use dummy credentials.
- Use a local mock server or an offline request example; inspect headers and response with a chosen HTTP client.
- Gate: explain 400, 401, 403, 415 and 500 as general HTTP meanings (actual API semantics depend on its documentation); distinguish invalid JSON from auth failure. No live assessment calls.

### Phase C — Security fundamentals

#### Lesson 07 — Bytes, Hashing & HMAC

- str vs bytes; UTF-8/ASCII; byte order; hex and Base64 representations.
- Hash digest vs encryption vs HMAC; shared-secret authentication; SHA family and why HMAC uses a key.
- Practice: compare public HMAC test vectors or unrelated toy messages with dummy keys using standard-library hashlib/hmac.
- Gate: explain why changing a message or key changes the digest; distinguish raw bytes from hexadecimal text; verify a public test vector independently.

#### Lesson 08 — RFC Reading, HOTP & TOTP

- Read terminology, MUST/SHOULD, algorithm sections, reference implementations, test vectors and errata.
- Start with HOTP (counter-based), then TOTP (time-based). Explain Unix time, time step, counter encoding, dynamic truncation, decimal formatting, leading zeros, clock skew and time-window boundaries.
- Use only public standards examples with test-vector parameters. Do not combine real assessment-specific parameters or generate its password.
- Gate: reproduce a published vector locally, explain each intermediate representation and consult errata when results disagree. Distinguish public-vector success from live service acceptance.

### Phase D — Independent integration

#### Lesson 09 — Local Mock Submission & Troubleshooting

- Integrate a fictional JSON request and dummy authentication against a local mock endpoint; separate test fixtures from production credentials.
- Diagnose malformed JSON, header mismatch, expired test credentials, clock skew, network errors and server-side failures. Read logs without exposing secrets.
- Build a personal checklist from the actual assessment instructions independently: permitted language, file format, restrictions, confidentiality, submission method and documented success condition.
- Gate: learner independently completes a local mock exercise and explains failures and fixes. The real assessment and submission remain the learner's own work. Do not treat a successful HTTP response as proof of code correctness or hiring outcome.

## 4. Coaching protocol / 每节课的固定节奏

1. Establish the lesson's objective and connect it to prior knowledge.
2. Explain the mental model and each new syntax element; use small fictional examples with concrete variable states.
3. Demonstrate a manual trace or request/response diagram; derive complexity for algorithm exercises where meaningful.
4. Ask 2–4 focused comprehension questions; wait for answers and grade each separately.
5. Give a distinct independent practice task with a minimal scaffold and representative tests; wait for the learner's attempt.
6. Review using the hint ladder (location → counterexample → principle → optional worked explanation for unrelated practice only). Label executed tests vs manual reasoning.
7. Record evidence locally or in an explicitly requested private archive, then advance on request. Never put personal progress in this repository.

## 5. Exit criteria & handoff

- Python: independently parse bounded input, explain functional operations and recursive stack behavior, test a constrained practice program and defend complexity.
- Engineering: explain secret-link visibility, construct a local HTTP request, distinguish common response failures.
- Security: distinguish bytes/encoding/hash/HMAC, explain HOTP/TOTP, read RFC errata and reproduce a public vector.
- Integrity: independently handle any real assessment and submission; keep tokens, credentials and solutions private.
- Status vocabulary follows SKILL.md (EXPLAINED, QUIZ PASSED/PARTIAL/NOT TAKEN, CODE SUBMITTED/REVIEWED/PASSED/NOT ATTEMPTED, SAVED/NOT SAVED). Use NOT RUN or UNKNOWN where appropriate; do not infer completion.

## 6. Public learning references

- [NeetCode Roadmap](https://neetcode.io/roadmap)
- [Python tutorial](https://docs.python.org/3/tutorial/)
- [Python built-in functions](https://docs.python.org/3/library/functions.html)
- [Python sys](https://docs.python.org/3/library/sys.html)
- [Python hashlib](https://docs.python.org/3/library/hashlib.html) and [hmac](https://docs.python.org/3/library/hmac.html)
- [GitHub: Creating gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/creating-gists)
- [RFC 7617 — Basic HTTP Authentication](https://www.rfc-editor.org/rfc/rfc7617) (updates the Basic scheme from RFC 2617)
- [RFC 4226 — HOTP](https://www.rfc-editor.org/rfc/rfc4226)
- [RFC 6238 — TOTP](https://www.rfc-editor.org/rfc/rfc6238) and [errata](https://www.rfc-editor.org/errata/rfc6238)

References are starting points, not a substitute for reading the current assessment instructions or independently exploring the standards.
