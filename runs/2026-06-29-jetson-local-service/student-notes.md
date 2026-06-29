# Student Notes

## What Was Confusing

1. `ProxyJump` and nested SSH are not equivalent from a student's point of view. `ProxyJump` used the local machine's key for the final Jetson login and failed. Nested SSH from the gateway succeeded.
2. The Jetson already had a working CLI build, but `llama-server` was missing. A student might assume all llama.cpp tools are built together.
3. Building `llama-server` pulled in Web UI asset generation. This created a long `npm` and `vite` step that is easy to mistake for a hang.
4. API success did not mean the model answer was correct. The 0.5B Q4 model returned a fluent but wrong explanation.
5. The service had to be stopped and the port checked after the test. Otherwise later students may hit a port conflict.

## Course Feedback

1. In the Jetson chapter, explicitly show gateway access as two alternatives: `ProxyJump` and nested SSH.
2. In the local service chapter, add a `PORT` variable and a port preflight before starting `llama-server`.
3. Tell students that `llama-server` may need a separate target build even after CLI tools work.
4. Warn that current llama.cpp server builds may compile Web UI assets and take several minutes.
5. Add a validation rule: API smoke test, timing, and output quality are three separate conclusions.
