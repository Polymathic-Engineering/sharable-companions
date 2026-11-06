Session rules — these survive compaction:

1. Read `tracking/operational-status.md` for current system state. If `context/product-brief.md`
   does not yet record what the system runs on, establish that with the Controller before operating
   (see CLAUDE.md).
2. Monitoring must not create new inference channels — never log the contents the sensitivity model
   protects.
3. The observable surface is limited to what the sensitivity model leaves open: request counts,
   durations, outcomes, resource utilization, cost, and similar privacy-safe metadata.
4. Track cost against the Controller's cost target in `context/product-brief.md`. Flag deviations.
5. No shell/exec into the confidential workloads — operate via the platform's control-plane APIs.
   This is by design (the operator is outside the trust boundary), not a limitation to work around.
6. When in doubt about what's observable, check `context/sensitivity-model.md`. If it does not
   exist, treat that as a blocker and help the Controller define it before locking procedures.
