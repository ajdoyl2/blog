# From Food Truck to Franchise: A Data Architecture Story

### *Two kitchens, two paths — one that scaled by design, and one that scaled by accident.*

-----

## The Premise

Every great restaurant empire started with someone who could cook. A food truck, a flat-top grill, a short menu, and a line around the block. Nobody was thinking about supply chain logistics or franchise compliance manuals. They were thinking about tomorrow's lunch rush.

Data organizations follow the same arc. They start with someone who can query — a single analyst with a database and a deadline. The work is good, the stakeholders keep coming back, and nobody is thinking about data architecture. They're thinking about Monday's board deck.

But then demand grows. And what separates the organizations that scale gracefully from those that don't isn't talent or budget — it's *sequencing*. Investing in the right capability at the right time, driven by actual constraint rather than industry trend.

This essay traces two parallel stories. The first is the *ideal path* — a food operation that scales from truck to global brand by investing just ahead of demand. The second is *what actually happens* at many large, complex institutions — a path shaped less by strategy than by organizational inertia, vendor momentum, and the gravitational pull of decisions made a decade ago.

Both stories are real. One is a blueprint. The other is a mirror.

-----

## Stage 1: The Food Truck

5 people. One grill. Pure hustle.

### The Ideal Path

A food truck is a marvel of constraint. Forty square feet, five menu items, one person doing everything. The owner buys ingredients at the market each morning and serves lunch by noon. There is no inventory system — there's a person who looks in the cooler and says, "We need more buns."

In data terms, this is the founding analytics team. Five people with direct access to a production database, building reports in spreadsheets and answering questions by end of day. The "data warehouse" is a shared drive. The "catalog" is institutional memory. Everything moves fast because everything fits in one person's head.

What works: Speed. Intimacy with the data. Zero coordination overhead. A customer asks for jalapeños and the cook just adds jalapeños.

What triggers the next investment: The line gets too long. More requests than five people can handle. Stakeholders start building their own spreadsheets because the team can't respond fast enough. The answer isn't better tooling — it's more people.

### What Actually Happened

At a large regulated financial institution — call it *the Firm* — the early analytics function looked exactly like this. A small team, maybe five analysts, working directly with IBM DB2 databases and SAS. The tools were powerful for their era. SAS could handle statistical modeling, data manipulation, and reporting in a single environment. DB2 was rock-solid for transactional data. WebFocus generated the reports that leadership consumed.

At this stage, the Firm and the ideal path were indistinguishable. Small team, direct access, fast turnaround. The cook and the kitchen matched the demand.

Where the paths begin to diverge: In the ideal path, the next investment is driven by a specific constraint. At the Firm, the next investment was driven by headcount growth — the team doubled, then tripled, without a corresponding investment in the systems that would let a larger team work coherently.

-----

## Stage 2: The Brick-and-Mortar Restaurant

10–30 people. A real kitchen. Defined roles.

### The Ideal Path

The food truck succeeds and the owner signs a lease. Now there's a walk-in fridge, a six-burner range, prep stations, and defined roles — a sous chef, a line cook, front-of-house staff. The menu expands. Ingredients are bought in bulk. A POS system tracks sales. A recipe book ensures consistency.

The critical investments at this stage are:

- A walk-in fridge (centralized data warehouse) — because you can't keep running to Costco every morning
- Kitchen stations (team specialization) — because one person can't prep, cook, plate, and serve simultaneously
- A recipe book (documented transformation logic) — because two cooks making the sauce differently is how you lose regulars
- A health inspection process (basic governance) — because when a regulator asks where a number came from, you need an answer

Each investment solves a specific, current problem. The warehouse exists because queries are killing the production database. Specialization exists because analysts shouldn't be managing infrastructure. Documentation exists because numbers are starting to conflict.

### What Actually Happened

The Firm's analytics team grew from 5 to 10, then to 30. But the kitchen didn't change. DB2 remained the primary data source. SAS remained the primary tool. The team added more analysts without adding the infrastructure that a larger team requires — no centralized transformation layer, no version control, no environment separation.

Then two things happened that shaped the next decade.

First, Hadoop. When Google's MapReduce papers led to the open-source Hadoop ecosystem, the Firm invested in an on-premise data lake. This was a reasonable bet at the time — every large institution was making it. But a data lake without a data strategy is a walk-in fridge without a chef: ingredients go in, and nobody organizes them, labels them, or throws out what's expired. The lake became a swamp.

Second, Tableau. The Firm adopted Tableau to modernize its reporting layer, replacing some WebFocus usage. But it didn't retire WebFocus — it kept both. This is the restaurant equivalent of installing a new POS system while keeping the old cash register plugged in "just in case." Now there are two systems, two sets of reports, and two sources of truth. The cognitive load on the team doubled without a corresponding increase in capability.

The divergence: In the ideal path, the restaurant invests in a walk-in fridge *and reorganizes the kitchen around it*. At the Firm, the fridge was installed in the parking lot (Hadoop), and the kitchen layout didn't change. The team kept cooking with the same tools in the same way, just with a large, underutilized cold storage facility out back.

-----

## Stage 3: The Restaurant Chain

30–100 people. Multiple locations. The system becomes the product.

### The Ideal Path

The single restaurant succeeded. Now there are five locations, then ten. The founder can no longer be in every kitchen. The operation needs systems that produce consistent results without direct supervision:

- A commissary kitchen (centralized transformation layer, mature dbt project) — prep the sauces once, distribute to every location
- Standardized recipes (semantic layer, governed metric definitions) — "revenue" means one thing, everywhere, always
- Quality control (automated testing, data observability) — catch the problem before the customer does
- Regional managers (data product owners embedded in business domains) — the central team can't understand every domain deeply enough
- Kitchen management software (orchestration — Airflow, Dagster) — you can't coordinate 200 recipes across 10 kitchens on whiteboards

The key insight at this stage: the product is no longer the food. The product is the system that produces the food. The founder's job shifts from cooking to designing systems that let other people cook consistently.

This is also where the *demand-supply feedback loop* intensifies. When you give business units self-service dashboards, they don't just consume the same reports faster — they start asking entirely new questions. Demand doesn't grow 2x; it grows 10x and diversifies. Every supply-side investment reshapes the demand curve.

### What Actually Happened

The Firm's analytics team grew from 30 to 100. One hundred analysts. But the organizational model didn't evolve to match.

Here's what the kitchen looked like:

No commissary kitchen. There was no centralized transformation layer. Each analyst built their own queries, their own logic, their own definitions. One hundred people, one hundred versions of "active customer."

No data engineering capability. Instead of building an internal data engineering function, the Firm outsourced this to IT delivery partners. This is the restaurant equivalent of hiring a general contractor to run your kitchen — they can build things, but they don't know what the food should taste like, they don't eat at the restaurant, and every request goes through a project manager with a six-to-nine-month timeline.

Six to nine months. That's how long it took to move an analytics asset from a development environment into production. No stakeholder was willing to wait. So nobody did.

Alteryx as the escape valve. The Firm adopted Alteryx, originally for its ability to perform cross-database joins — connecting DB2, the Hadoop lake, flat files, and other sources that the institutional infrastructure couldn't bridge. But Alteryx quickly became something it was never designed to be: the de facto data engineering platform.

This is a critical moment in the story, and it deserves careful attention because it illustrates a pattern that repeats across large institutions. *When the formal system can't serve demand, the informal system fills the gap.*

Analysts couldn't get data engineering support for months, so they became their own data engineers — using Alteryx. They couldn't promote code to production through normal channels, so they ran Alteryx workflows on local machines or shared servers, outside of version control, outside of monitoring, outside of any governance framework.

The result: hundreds of undocumented, ungoverned, unversioned Alteryx workflows performing critical data transformations across the institution. Analysts were doing engineering work without engineering tools, engineering practices, or engineering oversight. Not because they wanted to — because the system gave them no alternative.

In restaurant terms: the kitchen was so backed up that the waitstaff started cooking in the dining room on hot plates. The food got served. It wasn't safe, consistent, or scalable — but customers stopped complaining about wait times.

The divergence deepens. At Stage 3, the ideal path organization has invested in systems that let it scale — orchestration, quality frameworks, domain ownership, governed self-service. The Firm invested in *headcount* without the corresponding systems. It went from 5 to 100 people while keeping essentially the same infrastructure architecture, adding tools (Hadoop, Tableau, Alteryx) without integrating them into a coherent platform.

The result is an analytics organization that is simultaneously overstaffed and underserved. One hundred analysts, each individually capable, collectively undercut by the absence of shared infrastructure, shared definitions, and a credible path from development to production.

-----

## Stage 4: Mass Production

100–300 people. Industrial scale. The system must be engineered.

### The Ideal Path

Mass production is a fundamentally different enterprise from running restaurants. Think of the leap from a burger chain to a company that produces millions of frozen patties per day for grocery stores, cafeterias, and quick-service restaurants nationwide.

The factory floor looks nothing like a kitchen. There are industrial mixers, conveyor belts, flash-freezing tunnels, and robotic packaging lines. The "chef" is now a food scientist. The "recipe" is a manufacturing specification with tolerances measured in fractions of a percent. Quality control isn't a mystery shopper — it's a laboratory with spectrometers and statistical process control charts.

The operation is now capital-intensive rather than labor-intensive. A food truck succeeds on the talent of the cook. A factory succeeds on the design of the system. Individual skill matters less than process reliability, and the cost of failure is measured not in one bad meal but in product recalls and brand damage.

At this stage, the data organization looks like:

| Factory Component            | Data Platform Equivalent                                                                        |
| ---------------------------- | ----------------------------------------------------------------------------------------------- |
| Automated production lines   | Fully orchestrated pipelines with SLAs, alerting, and self-healing                             |
| QC laboratory                | Automated data quality at every layer — schema validation, drift detection, business rule testing |
| Manufacturing specifications | Data contracts between producers and consumers, versioned and enforced                          |
| Food scientists              | Data architects, platform engineers, ML engineers                                               |
| Statistical process control  | Data observability — freshness tracking, volume anomaly detection, SLA monitoring              |
| ERP system                   | Internal platform tooling — metadata management, cost allocation, usage tracking               |
| USDA/FDA compliance          | Regulatory governance — lineage, access controls, retention, audit trails                      |
| Supplier certification       | Upstream data quality agreements, source system SLAs                                           |

The organizational model has shifted completely. There's a Chief Data Officer. A VP of Platform. Domain data product owners embedded in each business line. A data quality engineering function. FinOps for cost management. In regulated industries, there are model risk management teams, data stewards, and regulatory reporting leads.

The key characteristic of this stage: data is treated as infrastructure, with the same operational rigor as the network, the core transaction system, or the trading platform. SLAs are measured and met. Incidents are tracked and postmortem'd. Capacity is planned. Costs are allocated to the business lines that consume them.

### What Actually Happened

The Firm's analytics team grew to 300 people. Three hundred. And then the institution adopted Azure Synapse.

On paper, this was the right move. Synapse is a modern cloud analytics platform capable of serving as the foundation for a mature data ecosystem. It provides SQL analytics (dedicated and serverless pools), Spark compute, data integration (Azure Data Factory), and connectivity to the broader Azure ecosystem.

In practice, the implementation revealed a structural problem that no technology can solve: the absence of data strategy and data leadership.

Here is what happened:

Synapse was configured for software development, not data work. The IT organization that provisioned the environment designed it the way they design application infrastructure — as isolated resources for isolated project teams. Each data team received its own instance of Azure Data Factory. Its own dedicated SQL pool. Its own serverless pool. Its own Spark pool. No shared resources. No shared metadata. No shared orchestration layer.

This is the equivalent of building a food production facility where every product line has its own refrigerator, its own oven, its own loading dock, and its own quality lab — and none of them connect. The building exists, but the factory doesn't.

The data lake migrated to the cloud — structurally unchanged. When Synapse arrived, the on-premise Hadoop data lake began its migration to Azure. In principle, this was the moment to redesign the storage layer — to organize data into zones (raw, curated, consumption), implement a catalog, and establish access patterns suited to analytical workloads.

Instead, the data was moved as-is into a single Azure Storage account with a container for each table. Just under 50,000 containers. Each system of record has a requestable entitlement at the individual person level — meaning if an analyst needs access to a new data source, they submit a request for themselves, for that specific source, and wait for approval.

This is the equivalent of moving your cold storage from an old warehouse to a modern facility, but keeping every ingredient in its own locked mini-fridge, each requiring a separate key issued to each cook individually. The building is new. The organizational principle is the same one that made the old warehouse unusable.

Azure Data Factory was provisioned but not configured. ADF is a powerful orchestration and integration tool, but out of the box it's an empty canvas. Without configured linked services, parameterized pipelines, a triggering strategy, a monitoring framework, and an error-handling pattern, ADF is a toolbox with no instructions. The IT organization handed data teams the toolbox and told them to build a house.

The data teams — 300 analysts whose background was in SAS, SQL, Alteryx, and Tableau — were now expected to become cloud data engineers. To design pipeline architectures. To write Spark code. To configure infrastructure-as-code. To build in an environment designed by and for software engineers, without software engineering training, practices, or support.

No data strategy. There is no institutional document that articulates what the data platform is for, who it serves, what "good" looks like, or how investment decisions should be made. There is no prioritized backlog of data capabilities. There is no architecture target state that teams are building toward. Each team is making independent decisions about tooling, patterns, and standards — or more commonly, making no decisions at all and defaulting to what they know (SAS, Alteryx, manual processes).

No data leadership. There is no Chief Data Officer. No VP of Data Platform. No one whose job title, authority, and incentive structure aligns with building a coherent data ecosystem. Data is everyone's responsibility and no one's authority.

Data engineers arrived — into a vacuum. Recently, the Firm began placing data engineers on data product owner teams. This was the right instinct — the recognition that analysts shouldn't be doing engineering work with analyst tools. But the engineers arrived without a shared platform, without agreed-upon patterns, and without coordination across teams.

Each team's engineers are independently trying to figure out their tools and their strategy. They're making reasonable but divergent decisions — different naming conventions, different pipeline patterns, different approaches to testing — because no one has the authority to establish shared standards and no platform exists to enforce them.

This is the restaurant equivalent of hiring professional line cooks and placing one in each franchise location, but giving them no recipe book, no commissary kitchen, and no executive chef. Each cook is talented. Each cook is solving the same problems differently. And six months from now, the institution will have a dozen incompatible "standards" instead of one.

The Databricks question. The data practitioners — the engineers and the more technically fluent product owners — have correctly diagnosed that the Synapse implementation is a significant part of the problem. The isolated resource model, the lack of shared infrastructure, the warehouse capacity limitations that can't keep pace with stakeholder demand for larger and more complex analytical workloads — these are real constraints, and they're getting worse.

The emerging consensus among practitioners is to migrate from Synapse to Databricks. IT has offered a loose commitment of six to twelve months. In the meantime, the teams are caught in a holding pattern: stakeholder expectations continue to grow, data volumes continue to exceed what their allocated warehouse capacity can handle, and every request for more space, better orchestration tooling, or dbt Cloud for managed transformations gets caught in procurement and provisioning cycles that weren't designed for iterative, demand-driven investment.

The most urgent tactical challenge sits right here: the Alteryx workflows must be migrated. Hundreds of them, built over years, many duplicating logic across teams. The engineers know these need to be refactored — not just moved but fundamentally redesigned from ETL (extract, transform, load) to ELT (extract, load, transform), pushing transformations into the warehouse where they can be version-controlled, tested, and optimized.

If the Alteryx workflows are simply replicated in a cloud tool without this refactoring, the compute costs will be staggering. Moving bad patterns to expensive infrastructure doesn't fix the patterns — it just makes them more expensive.

But refactoring requires a warehouse with capacity, an orchestration layer that works, and a transformation framework like dbt. The teams are requesting these tools. The requests are in flight. And the stakeholders who need the data don't care about any of this — they care that the dashboard is late and the numbers don't match.

This is the Firm's current moment: caught between a legacy architecture that can't scale and a modern architecture that isn't ready yet, with a migration horizon that keeps receding and stakeholder patience that isn't infinite.

In restaurant terms: the franchise has committed to building a new central kitchen, but the permits are six to twelve months out. In the meantime, the existing kitchens are out of refrigerator space, the health inspector is coming, and customers are still ordering dinner. The waitstaff promising "the new kitchen will fix everything" is not a strategy. It's a prayer.

The compounding effect. Zoom out and look at what the Firm's data landscape actually contains:

- IBM DB2 — still the primary transactional data source, still feeding critical processes
- SAS — still in production, still running scheduled jobs that generate regulatory and operational reports
- WebFocus — still generating reports, years after Tableau was positioned as its replacement
- Hadoop data lake (migrated to Azure Storage) — ~50,000 containers in a single storage account, one per table, with per-person entitlements; structure unchanged from on-premise
- Tableau — the primary visualization layer, but without a governed semantic layer feeding it
- Alteryx — hundreds of ungoverned workflows performing critical transformations, many running on local machines or shared servers without version control
- Azure Synapse — provisioned but not operationalized, configured for software teams, with isolated instances per data team and no shared infrastructure

Seven major platforms. No integration strategy. No data contracts between them. No lineage connecting them. No single team or leader accountable for making them work together.

In restaurant terms: the Firm didn't build a factory. It built a food court — a dozen independent kitchens under one roof, each with its own suppliers, its own recipes, its own quality standards, and its own version of what "chicken" means. The building is expensive. The output is inconsistent. And nobody has the authority to redesign the floor plan.

### The Core Comparison

| Dimension              | Ideal Path at Stage 4                                                            | What Actually Happened                                                                                                                |
| ---------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Investment trigger** | Specific constraint drives targeted investment                                   | Vendor/IT cycle drives technology adoption independent of data strategy                                                               |
| **Platform architecture** | Integrated, shared, with governed access and defined domains                  | Isolated instances per team, configured by IT for IT patterns; cloud data lake is 50K containers with per-person access              |
| **Data engineering**   | In-house capability, embedded with domain expertise                              | Engineers recently placed on teams, but working independently without shared standards or platform                                    |
| **Transformation layer** | Centralized framework (dbt, governed SQL) with CI/CD                           | Fragmented across SAS, Alteryx, ad-hoc SQL with no version control; dbt Cloud requested but not yet provisioned                      |
| **Orchestration**      | Managed platform (Airflow/Dagster) with dependency management and SLAs          | ADF provisioned but unconfigured; orchestration tooling requested but not yet available                                              |
| **Governance**         | Formalized ownership, lineage, quality testing                                   | Absent. No data owners. No contracts. No quality framework                                                                            |
| **Strategy**           | Written, communicated, funded data strategy with executive sponsorship           | None                                                                                                                                  |
| **Leadership**         | CDO or equivalent with authority and budget                                      | No dedicated data leadership                                                                                                          |
| **Legacy management**  | Deliberate migration plan; legacy carried as known, costed technical debt        | Databricks migration discussed with 6-12 month IT timeline; Alteryx flows need ETL-to-ELT refactoring before migration or costs explode |
| **Path to production** | CI/CD, automated testing, environment promotion in days                          | 6-9 month waterfall process through IT, bypassed by analysts running Alteryx locally                                                 |

-----

## Stage 5: Globalized Production

The Data Equivalent: Enterprise-Scale, Multi-Domain, AI-Native

### The Ideal Path

The final stage isn't just "bigger." It's qualitatively different — the way Nestlé or Unilever is different from a national frozen food brand.

Global production means:

- Multiple regulatory jurisdictions with conflicting requirements
- Distributed operations across regions and business lines
- Real-time coordination — supply disruptions in one region affect another
- Sophisticated R&D as a competitive advantage
- End-to-end traceability from source to consumer
- Sustainability and cost accountability at the platform level

At the most mature data organizations, the platform has become an operating system for the enterprise. Data isn't a support function; it's the medium through which the business operates. Real-time fraud scoring on every transaction. Dynamic pricing models. Algorithmic credit decisioning. AI-powered customer interactions grounded in institutional data.

The tooling at this stage is less about individual products and more about platform architecture:

- Data sovereignty — region-aware storage and processing for regulatory compliance
- ML/AI platform — feature stores, model registry, experiment tracking, production model serving
- Streaming architecture — millions of events per second with sub-second latency
- Data mesh at scale — federated domain ownership with centralized standards and interoperable products
- Automated lineage — column-level tracing from source transaction to regulatory filing
- Self-healing infrastructure — auto-scaling, auto-remediation, AI-powered anomaly detection

### Where the Firm Stands

The Firm is not at Stage 5. It is not at Stage 4. Measured by its tooling — Synapse, ADF, Spark, a cloud data lake — it has the *equipment* for Stage 4. Measured by its organizational maturity — no data strategy, no data leadership, no governed path from development to production, no data contracts, no quality framework, fragmented ownership — it is operating at late Stage 2 with Stage 4 price tags.

This is the most expensive possible position. The institution is paying for industrial equipment and running it like a food truck. The cloud infrastructure bill is growing. The legacy tooling bill hasn't shrunk. The headcount is 300 and climbing. And the output — the quality, consistency, and trustworthiness of the data products reaching stakeholders and regulators — is no better than it was with 30 people and SAS.

What makes the current moment particularly precarious is the convergence of three dynamics:

The practitioners see the problem clearly. The recently arrived data engineers and the more technically fluent product owners have correctly identified the Synapse implementation as a constraint. They're requesting the right tools — dbt Cloud, proper orchestration, warehouse capacity. They're advocating for the right migration — Databricks, with its unified lakehouse architecture, better Spark integration, and collaborative workspace model. They understand that the Alteryx workflows must be refactored, not just moved, and that the refactoring sequence matters.

But the timeline isn't theirs to control. The Databricks migration is six to twelve months away at best — an IT commitment, not a data strategy commitment. Procurement, provisioning, security review, and configuration will all flow through the same IT organization that configured Synapse as a software development platform. Without data leadership at the table during those decisions, there is every reason to expect history will repeat: a powerful tool, configured for the wrong audience, handed to data teams without the patterns, permissions, or platform design they need to use it effectively.

And stakeholders aren't waiting. The analytical workloads keep growing. Data volumes exceed allocated warehouse capacity. New use cases emerge faster than the infrastructure can accommodate them. Every month the teams spend in the holding pattern — waiting for Databricks, waiting for dbt, waiting for orchestration tooling — is a month where Alteryx workflows continue to multiply, technical debt compounds, and the eventual migration becomes larger and more expensive.

This is not a technology problem. The Firm does not need another tool. It needs three things:

1. Data leadership — a senior executive with authority, budget, and accountability for the data ecosystem as a whole
2. Data strategy — a written, funded, prioritized plan that articulates the target state, the migration path, and the sequencing of investments
3. Organizational redesign — data engineering as an in-house capability, data product ownership as a defined role, and a credible path from development to production that doesn't take nine months or require analysts to become unlicensed engineers

-----

## The Through-Line: When to Invest and In What

If there's one lesson the food industry teaches about scaling, it's this: invest in the constraint, not in the aspiration.

A food truck owner doesn't buy a walk-in freezer because they want to be a restaurant someday. They buy it because they're losing $200 a day in spoiled ingredients and the freezer pays for itself in three months.

A restaurant chain doesn't build a commissary kitchen because they read about it in a trade magazine. They build it because location #7 is making the sauce wrong and customers are complaining.

| Don't invest because…                        | Invest because…                                                                                                                     |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| "Everyone's moving to the cloud"             | Your on-premise infrastructure can't scale to meet the analytical workload and the cost of ownership exceeds cloud alternatives    |
| "We need a modern data stack"                | Your analysts spend 40% of their time reconciling conflicting definitions and the other 60% waiting for IT to move things to production |
| "AI will transform our industry"             | You have a validated use case with quantified impact and the data foundation to support it — which means you need the foundation first |
| "We should adopt Synapse/Databricks/Snowflake" | You have a data strategy that defines what the platform needs to do, and you've selected the tool that best serves that strategy   |
| "We need more data engineers"                | You know exactly what those engineers will build, how it will be governed, and who will own the output                              |

The Firm's history is a case study in what happens when this principle is inverted. Hadoop was adopted because it was the industry trend, not because a specific workload demanded it. Synapse was provisioned because the cloud migration was happening, not because a data strategy required it. Tools were added but never integrated, and nothing was ever retired.

Every un-retired tool is a tax. Every ungoverned platform is a liability. Every analytics professional doing engineering without engineering tools is a risk — to data quality, to operational resilience, and in a regulated industry, to compliance.

-----

## The Demand-Supply Feedback Loop

There's a dynamic that runs through every stage of this story: consumer demand doesn't just grow — it reshapes itself in response to supply.

When the food truck starts offering delivery, it doesn't just get more of the same orders. It gets a different kind of demand — higher volume, lower margin, time-sensitive.

The data equivalent plays out at every stage:

Give stakeholders self-service dashboards and they don't consume the same reports faster — they ask entirely new questions. Demand grows 10x and diversifies.

Build a curated, trusted data layer and business teams embed data into operational processes — marketing automation, credit decisioning, compliance monitoring. Data is now on the critical path. Downtime becomes an outage.

Enable real-time data and ML and you don't just speed up existing use cases — you enable entirely new business models that create demand the original architecture never anticipated.

This is why data platform evolution never feels "done." Every supply-side investment changes the demand curve. You're not running to catch a fixed target — you're running on a treadmill that accelerates every time you improve.

At the Firm, this dynamic is suppressed. Because the supply side is so constrained — no credible path to production, no governed self-service, no integrated platform — demand hasn't been allowed to mature. Stakeholders have learned not to ask for things the system can't deliver. They've adapted to the limitations, which means they've stopped imagining what's possible.

This is the most insidious cost of underinvestment. It's not just what the platform can't do today — it's the questions that will never get asked because nobody believes the platform could answer them.

-----

## Closing: The Cook and the Factory

At the end of the ideal journey, something important has been lost, and something important has been gained.

The food truck cook — who tasted every dish, knew every customer, could improvise a special based on what looked good at the market — that person is gone. In their place is an organization: systematic, reliable, scalable, auditable. The food is consistent. The compliance posture is strong. The cost structure is understood.

But the best food companies maintain pockets of that original magic. Test kitchens. Empowered chefs at individual locations. Limited-run specials. They know that the factory is a means to an end, and the end is still feeding people well.

The best data organizations do the same. Sandboxes for exploration. Innovation sprints. Embedded analysts who sit with the business and move fast. Self-service tools that let people ask their own questions. They know that the platform is a means to an end, and the end is still decisions — faster, better, more informed decisions by more people, more often.

The Firm has 300 talented people who want to cook. It has recently hired professional line cooks — data engineers — and placed them in the kitchens. Those engineers can see exactly what needs to happen. They're requesting the right equipment. They're advocating for the right redesign. They're trying to refactor the recipes while still serving dinner.

What they lack isn't skill, ambition, or even diagnosis. It's a kitchen designed to let them work, a timeline they can control, and an executive willing to say: *this is what we're building, this is why, this is the order in which we build it — and we're not waiting twelve months to start.*

The Databricks migration may be the right destination. But a destination without a strategy is just a dot on a map. The teams need the tools now — dbt, orchestration, warehouse capacity — not as part of a future-state architecture, but as prerequisites for surviving the present state.

The Alteryx workflows won't refactor themselves. The stakeholders won't stop ordering dinner. And every month spent in the holding pattern makes the eventual migration more expensive and more fragile.

The food truck was honest. It knew what it was. The factory must be equally honest about what it needs to become — and about the cost of waiting.

Start with the strategy. Then build the kitchen to match.

-----

*Appendix: Maturity Snapshot*

| Capability         | Stage 1 (Truck)        | Stage 2 (Restaurant)  | Stage 3 (Chain)                   | Stage 4 (Factory)                           | Stage 5 (Global)                              | The Firm Today                                                                                           |
| ------------------ | ---------------------- | --------------------- | --------------------------------- | ------------------------------------------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Data storage       | Prod DB / flat files   | Cloud warehouse       | Multi-layer warehouse (medallion) | Enterprise lakehouse with domains           | Multi-region, sovereign                       | DB2 + cloud data lake (50K containers, per-person entitlements) + Synapse (isolated, capacity-constrained) |
| Transformation     | Excel / ad hoc SQL     | Organized SQL scripts | dbt with CI/CD                    | dbt + streaming transforms                  | Real-time + batch unified                     | SAS + Alteryx + ad hoc SQL (no version control); dbt Cloud requested, not provisioned                   |
| Orchestration      | Manual / cron          | Scheduled jobs        | Airflow / Dagster                 | Managed platform with SLAs                  | Self-healing, event-driven                    | ADF provisioned but unconfigured; orchestration tooling requested                                        |
| Quality            | Trust the analyst      | Spot checks           | Automated dbt tests               | Full observability + data contracts         | Platform-as-a-service QA                      | None formalized                                                                                          |
| Governance         | Tribal knowledge       | Shared docs           | Catalog + ownership               | Federated governance + lineage              | Global policy engine                          | None                                                                                                     |
| Path to production | Immediate (no process) | Days (light review)   | Hours (CI/CD)                     | Minutes (automated promotion)               | Continuous deployment                         | 6-9 months (or bypassed entirely via Alteryx)                                                            |
| Engineering        | N/A                    | First engineer hire   | Embedded team                     | Platform engineering org                    | Global platform                               | Engineers recently placed on teams; no shared standards, tooling, or platform                            |
| Leadership         | Analyst-manager        | Analytics director    | VP of Analytics + domain leads    | CDO + platform VP + domain owners           | Global data board                             | No dedicated data leadership                                                                             |
| Strategy           | Implicit               | Emerging              | Written, funded                   | Enterprise-wide, multi-year                 | Board-level, tied to corporate strategy       | None; Databricks migration discussed (6-12 month IT timeline)                                            |

-----

*Inspired by [Farm-to-Table Data Architecture](https://github.com/zenzombie/zenzombie-blog/blob/main/content/essays/farm-to-table-data-architecture.md) by zenzombie.*
