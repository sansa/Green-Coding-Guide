---
title: "Green Coding Practices by Domain"
nav_order: 4
permalink: /domain-practices/
---

# 4. Green Coding Practices by Domain

Green coding principles apply across domains, but their practical impact depends on execution context. This section organises practices by domain, focusing on **dominant contributors to runtime energy footprint**. The practices below expand on the core principles from [Section 3](/core-principles/) and are intended to give practitioners a concrete starting point for each domain.

---

## 4.1 Frontend – Web Applications

Energy footprint drivers include JavaScript execution, rendering, and network activity. Because the same code runs on millions of user devices, frontend inefficiency is multiplied across the entire user base, making even small per-page improvements significant at scale (Manotas et al., 2016).

**Minimise unnecessary re-renders and layout thrashing**
In component-based frameworks, a state change in a parent can trigger re-renders in subtrees that are unaffected by the change. Each re-render involves diffing, DOM patching, style recalculation, and potentially layout, all of which is CPU work that generates heat on the user's device. Use memoisation, selector functions, and component splitting to ensure that only the components that need to update actually do. Similarly, avoid interleaving DOM reads and writes in JavaScript loops, which forces the browser to repeatedly recalculate layout (a pattern known as layout thrashing).

**Reduce bundle size through code splitting and lazy loading**
Larger JavaScript bundles require more parsing, compilation, and execution time, all of which consume energy before a single line of application logic runs. Code splitting ensures that users download and execute only the code needed for their current view. Lazy loading defers the rest until it is actually required. Audit bundle composition regularly using build analysis tools, and remove unused dependencies.

**Avoid chatty API interactions**
Multiple small, sequential API calls keep the network interface active and increase latency. Each network request also has a fixed energy overhead independent of payload size: initiating a connection, performing DNS resolution, and completing the TCP/TLS handshake. Consolidate related requests, use HTTP/2 multiplexing where available, and consider query languages (such as GraphQL) that allow the client to specify exactly the data it needs.

**Use caching and HTTP mechanisms effectively**
Redundant data transfer is one of the most avoidable sources of frontend energy consumption. Correct use of `Cache-Control` headers, ETags, and service workers can eliminate repeat fetches of unchanged resources entirely. Measure actual cache hit rates to confirm that caching is working as intended.

**Prefer CSS and hardware-accelerated animations over JavaScript loops**
JavaScript-driven animations run on the main thread and compete with layout and rendering. CSS transitions and the Web Animations API delegate work to the compositor thread, which is typically GPU-accelerated and does not block main thread execution. Use `transform` and `opacity` for animations, as these properties can be composited without triggering layout or paint.

---

## 4.2 Frontend – Mobile Applications

Mobile energy footprint is dominated by battery usage from the CPU, network radios, display, and sensors. The cost of waking the device from a low-power state is significant: a single unnecessary wakeup can consume as much energy as many milliseconds of normal CPU activity (Pathak et al., 2012).

**Reduce background activity and wakeups**
Each background wakeup forces the CPU and potentially the network radio out of a low-power sleep state. Consolidate background tasks using platform-provided APIs such as `WorkManager` on Android and `BGTaskScheduler` on iOS, which allow the OS to schedule work during periods when the device is already active or charging. Avoid independent timers and alarms for tasks that can tolerate batching.

**Batch network requests and support offline workflows**
The cellular radio (LTE/5G) is one of the most energy-intensive components in a mobile device and consumes significant power even in the tail period after a transmission ends, while waiting for further activity. Batching requests into fewer, larger interactions reduces the total time the radio spends in its active and tail states. Supporting offline workflows, by storing and replaying operations when connectivity is restored, further reduces unnecessary transmission.

**Optimise rendering and scrolling**
Off-screen rendering, excessive overdraw (drawing the same pixel multiple times per frame), and repeated layout passes drain battery directly. Use view recycling (e.g., `RecyclerView`, `LazyColumn`) to avoid rendering off-screen content. Reduce overdraw by flattening view hierarchies and removing opaque backgrounds that are hidden by foreground content.

**Use sensors conservatively and at appropriate precision**
GPS at full accuracy and high polling rate is among the most energy-intensive sensors on a mobile device. Where coarse location is sufficient, use `PRIORITY_LOW_POWER` or geofencing rather than continuous high-accuracy tracking. Apply the same principle to accelerometers, gyroscopes, and microphones: request the lowest sampling rate and precision that satisfies the functional requirement, and unregister listeners when they are no longer needed.

---

## 4.3 Backend – Services and APIs

Backend energy footprint is driven by CPU usage per request, data access patterns, and network communication volume. Because a single backend instance serves many concurrent users, inefficiencies compound rapidly with traffic growth.

**Reduce work per request**
Profile hot paths to identify operations that execute on every request. Eliminating or deferring even a single expensive database call, computation, or external service call from a high-traffic endpoint can yield significant aggregate energy savings across the server fleet. Treat per-request CPU time as a resource budget that should be defended as the codebase evolves. In a controlled study comparing API design variants, replacing an O(n²) nested-loop sort with an optimised built-in equivalent reduced power draw by approximately 23% and response time by 43% relative to the unoptimised baseline (Joof et al., 2025; Joof, 2025).

**Avoid N+1 queries and over-fetching**
Loading a list of entities followed by individual queries for each entity is one of the most common and avoidable energy waste patterns in backend development. Replace N+1 patterns with a single batch query or an eager-loading mechanism provided by the ORM or query builder. Equally important is selecting only the columns required, because fetching entire rows when only two fields are needed wastes I/O bandwidth and increases serialisation overhead.

**Use bounded, measured caching**
Caching is only beneficial if the hit rate justifies the memory footprint and staleness risk. An unbounded cache increases GC pressure and memory consumption; a cache with the wrong eviction policy may hold stale or rarely accessed entries. Instrument caches to measure hit rates, and set TTLs and size limits based on observed access patterns rather than defaults. Empirical results from API-level profiling show that introducing in-memory caching for repeated queries reduces power consumption by approximately 15% and response time by 36% relative to an uncached baseline (Joof et al., 2025; Joof, 2025).

**Minimise payload size and serialisation overhead**
JSON serialisation and deserialisation is CPU-intensive at scale. For internal service-to-service communication, binary formats such as Protocol Buffers or MessagePack can reduce both CPU time and transfer volume. For external-facing APIs, support response field filtering (e.g., via `fields` query parameters or GraphQL selections) so that clients are not forced to receive and discard data they do not use. Note that payload compression involves an explicit trade-off: GZIP encoding reduces network transfer but increases CPU consumption, and the net energy impact depends on the relative cost of transmission versus processing in the deployment environment (Joof et al., 2025; Joof, 2025).

---

## 4.4 Backend – Data Processing and Storage

Data pipelines consume energy through prolonged execution and large-scale I/O. Unlike interactive services, pipelines often run without a user waiting for a response, which means their energy consumption is less visible but no less real.

**Avoid full recomputation of unchanged data**
Re-running a pipeline over input data that has not changed since the last run is pure waste. Detect unchanged partitions using checksums, modification timestamps, or incremental computation frameworks (such as Apache Spark's structured streaming or dbt's incremental models), and skip them. Even a partial reduction in recomputation scope can substantially reduce total pipeline energy.

**Use incremental and checkpointed processing**
Long-running batch jobs that restart from scratch on failure waste all the energy invested in the failed run. Checkpointing, which periodically saves intermediate state to durable storage, enables the pipeline to resume from the last stable point rather than the beginning. The energy cost of writing checkpoints is typically small relative to the cost of a full restart.

**Apply data retention and lifecycle policies**
Storage energy grows with data volume: more data means more disk reads, more index maintenance, and more backup and replication overhead. Define and enforce retention policies that expire data when it is no longer needed for operational, analytical, or compliance purposes. Move cold data, such as infrequently queried historical records, to lower-power storage tiers rather than keeping it on high-performance primary storage.

**Align processing frequency with actual needs**
Running an hourly pipeline to produce results that are consumed daily is unnecessary work. Review the scheduling frequency of each pipeline in relation to the actual decision latency requirement of its downstream consumers. Where a delay is acceptable, prefer event-driven triggers over fixed schedules to avoid processing empty or minimal increments.

---

## 4.5 Backend – Infrastructure and Cloud

Infrastructure choices determine the baseline energy cost of running software before any request is served. Over-provisioned or poorly configured infrastructure wastes energy continuously, not just under load.

**Right-size compute resources**
An instance running at 5% average CPU utilisation is consuming roughly the same idle power as one running at 60%, while delivering far less useful work per watt. Profile actual resource utilisation across representative workload periods and right-size instances accordingly. Cloud environments make this reversible, so start conservatively and scale based on observed demand rather than theoretical peaks.

**Use scale-to-zero and serverless patterns for bursty workloads**
Services that are idle for significant portions of the day benefit from architectures that allow compute resources to scale down to zero when not in use. Serverless functions, container-based autoscaling (e.g., Knative, AWS Lambda, Google Cloud Run), and scheduled shutdown of non-production environments all reduce energy consumption during periods of low demand.

**Consider the carbon intensity of deployment regions**
The same workload produces different lifecycle carbon emissions depending on where it runs. Cloud regions powered predominantly by renewable energy sources have substantially lower carbon intensity than those relying on coal or gas. For workloads where latency requirements allow geographic flexibility, such as batch jobs, asynchronous processing, or data pipelines, prefer regions with lower carbon intensity. Most major cloud providers publish real-time or historical carbon intensity data for their regions.

**Enforce autoscaling and avoid sustained idle over-provisioning**
Fixed-capacity deployments that are sized for peak load remain fully provisioned and consuming energy during off-peak hours. Configure autoscaling policies that reduce replica count, instance size, or cluster node count during sustained low-traffic periods. Combine this with scheduled scaling for predictable traffic patterns (e.g., scaling down overnight for services with a known diurnal usage pattern).

---

## 4.6 AI Development – Training

AI training energy footprint is dominated by accelerator (GPU/TPU) runtime. Training a large model can consume hundreds to thousands of kilowatt-hours, which often exceeds the lifetime inference energy for many deployments (Strubell et al., 2019; Patterson et al., 2021).

**Right-size models and datasets**
Larger models and datasets do not always produce proportionally better results for a given task. Evaluate whether a smaller, fine-tuned, or distilled model meets quality requirements before committing to large-scale training. Similarly, audit training datasets for duplicates and low-quality samples that increase compute time without improving generalisation.

**Avoid redundant experiments**
Training runs with near-identical hyperparameters, or re-running an experiment that was interrupted without saving a checkpoint, are common sources of avoidable energy consumption. Use experiment tracking tools (such as MLflow or Weights & Biases) to record configurations and results, and establish convergence criteria before starting a run rather than judging retroactively.

**Apply early stopping and efficient training techniques**
Monitor validation metrics throughout training and stop when improvement plateaus rather than running for a fixed number of epochs. Mixed-precision training (FP16 or BF16) reduces memory bandwidth consumption and computation time per step on supported hardware, typically without material quality loss. Learning rate scheduling, gradient accumulation, and efficient attention implementations are further well-established techniques for reducing total training compute.

**Maximise hardware utilisation**
Low GPU utilisation during training, commonly caused by data loading bottlenecks, preprocessing on the CPU, or inefficient batching, means energy is consumed without productive work. Profile GPU utilisation and address pipeline stalls by prefetching data, increasing the number of data loader workers, or preprocessing and caching inputs offline.

---

## 4.7 AI Usage – Inference

Inference energy footprint scales with request volume. Unlike training, which occurs once per model version, inference runs continuously in production and its cumulative energy cost often exceeds the training cost over the deployment lifetime of a model.

**Use the smallest effective model**
Model complexity should be matched to task requirements. A large general-purpose model applied to a narrow, well-defined task is likely to be significantly over-specified. Techniques including quantisation (reducing numerical precision), pruning (removing low-weight connections), and distillation (training a smaller model to replicate a larger one's behaviour) can substantially reduce inference energy without proportional loss in output quality.

**Minimise prompt and input size**
In transformer-based models, self-attention complexity scales quadratically with input length. Longer prompts and larger context windows directly increase per-request compute cost. Trim irrelevant context, avoid verbose prompt templates, and evaluate whether retrieved context snippets can be shorter without reducing answer quality.

**Cache deterministic outputs and embeddings**
Identical or semantically equivalent queries that reliably produce the same output should be served from cache rather than triggering re-inference. Embedding computations for fixed reference documents should similarly be precomputed and cached rather than recalculated on each request. Semantic caching, which matches queries that are paraphrases of each other, can extend cache hit rates beyond exact-match scenarios.

**Batch requests where feasible**
Batching amortises the fixed cost of loading model weights from memory and allows the accelerator to operate at higher utilisation. For asynchronous or near-real-time use cases, accumulate requests into batches and process them together. Dynamic batching, which groups requests that arrive within a short time window, can achieve most of the throughput benefit without imposing significant additional latency.

---

## References

Manotas, I., Bird, C., Zhang, R., Shepherd, D., Jaspan, C., Sadowski, C., Pollock, L. and Clause, J. (2016) *An empirical study of practitioners' perspectives on green software engineering*. Proceedings of the 38th International Conference on Software Engineering (ICSE), pp.237–248.

Pathak, A., Hu, Y.C. and Zhang, M. (2012) *Where is the energy spent inside my app? Fine-grained energy accounting on smartphones with Eprof*. Proceedings of the 7th ACM European Conference on Computer Systems (EuroSys), pp.29–42.

Patterson, D., Gonzalez, J., Le, Q., Liang, C., Munguia, L.M., Rothchild, D., So, D., Texier, M. and Dean, J. (2021) *Carbon emissions and large neural network training*. arXiv preprint arXiv:2104.10350.

Pereira, R., Couto, M., Ribeiro, F., Rua, R., Cunha, J., Fernandes, J.P. and Saraiva, J. (2017) *Energy efficiency across programming languages: How do energy, time, and memory relate?* Proceedings of the 10th ACM SIGPLAN International Conference on Software Language Engineering, pp.256–267.

Strubell, E., Ganesh, A. and McCallum, A. (2019) *Energy and policy considerations for deep learning in NLP*. Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics (ACL), pp.3645–3650.

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
