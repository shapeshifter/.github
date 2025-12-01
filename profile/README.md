[Shapeshifter](https://lfenergy.org/projects/shapeshifter/) implements the Universal Smart Energy Framework for flexibility forecasting, offering, ordering, and
settlement processes. Additionally, ShapeShifter enables trading via DSO/TSO coordination platforms that also support
the protocol.

# Project overview

- [Specification + XSD](https://shapeshifter.github.io/shapeshifter-specification)
- [Java implementation](https://github.com/shapeshifter/shapeshifter-library-java)
- [Technical Steering Committee meeting notes](https://github.com/shapeshifter/tsc)

Shapeshifter enables a fast, fair, and low cost route to a smart energy future by delivering one common approach to
efficiently connect smart energy projects and technologies. Its market structure, roles, rules and tools for the
commoditization and trading of flexible energy usage work with existing and evolving energy markets.

Shapeshifter focuses specifically on the exchange of flexibility between aggregators (AGRs) and distribution system
operators (DSOs) or between aggregators and transmission system operations (TSOs). It describes the corresponding market
interactions between them to resolve grid constraints by applying congestion management or grid capacity management.

# Relation to other projects

- [OpenADR](https://www.openadr.org/) - Event-based demand/response signaling (price, reliability, flexibility).
- [OpenLEADR](https://openleadr.org/) - If OpenADR is suitable for you, this is an implementation for Python or Java you
  could use.
- [EVerest](https://lfenergy.org/projects/everest/) - An open-source firmware stack for EV charging stations, handling
  the low-level device logic (ISO 15118, PWM).
- [OCPP](https://openchargealliance.org/protocols/open-charge-point-protocol/) - The standard for communicating between
  EV charging stations and central management systems.

### Shapeshifter vs OpenADR comparison

| Feature               | Shapeshifter                                                                                                                                                                                   | OpenADR                                                                                                                                                                                |
|:----------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Primary Focus**     | **Market-based Flexibility Trading.** Focuses on the *trading* of energy flexibility (forecasting, offering, ordering, settlement). Designed for markets where flexibility is bought and sold. | **Demand Response signaling.** Focuses on sending signals (price, reliability, events) to devices to change consumption. Primarily a command-and-control or price-signaling mechanism. |
| **Workflow**          | Supports a full trading cycle: `D-prognosis` (forecast) $\rightarrow$ `Flex Offer` $\rightarrow$ `Flex Order` $\rightarrow$ `Settlement`. Assumes a negotiation or market process.             | Unidirectional or bi-directional signaling: "Shed load now" or "Price is high." Devices acknowledge, but it is less about negotiating a "deal" and more about reacting to a signal.    |
| **Roles**             | Defined by USEF roles: **Aggregator (AGR)**, **Distribution System Operator (DSO)**, **Transmission System Operator (TSO)**, **Balance Responsible Party (BRP)**.                              | **Virtual Top Node (VTN)** (usually utility/aggregator) and **Virtual End Node (VEN)** (usually the device/gateway).                                                                   |
| **Data Model**        | Explicitly models power profiles, baselines, and deviations for the purpose of financial settlement and grid congestion management.                                                            | Models events, reports, and opt-in/opt-out states.                                                                                                                                     |
| **Target Grid Level** | Often focused on **DSO (Distribution)** congestion management and TSO balancing, enabling local flexibility markets.                                                                           | Widely used for utility-to-customer programs (residential, commercial, industrial) for peak load reduction.                                                                            |

### Use Shapeshifter when:

* **Trading & Settlement are required:** You are building a system where flexibility is treated as a commodity that is
  forecasted, offered, ordered, and financially settled.
* **Congestion Management:** You are a DSO (Distribution System Operator) or working with one to manage local grid
  congestion using market-based mechanisms.
* **Complex Flexibility Products:** You need to express complex capabilities (e.g., *"I can reduce X MW for Y hours at
  price Z, but only if notified A hours in advance"*).

### When NOT to use Shapeshifter:

* **Simple Demand Response:** You just need to tell devices to turn off or reduce power during peak hours without a
  complex bidding/settlement process.
* **Device Control:** You are communicating directly with end devices (thermostats, heaters) that don't "negotiate"
  markets but just react to signals.
* **US Market Standard:** You are operating in the US or other markets where OpenADR is the established standard for
  utility demand response programs.
* **Broad Device Support:** You need off-the-shelf compatibility with a wide range of certified hardware (many devices
  are "OpenADR certified").

# Project Details

Shapeshifter is based on the market-based coordination mechanism (MCM) described by USEF. It facilitates the delivery of
value propositions (i.e. marketable services) to various market parties without imposing limitations on the diversity
and customization of the propositions.

This MCM is designed for all energy commodities and enables the market to optimize in time, capacity and power. The MCM
provides access, under equal conditions, for all stakeholders to a single integrated market. This unique approach aims
to deliver a future-proof market design.

In Shapeshifter, the general MCM phases are followed. However, processes in each phase are limited to the interactions
between the AGR and DSO or TSO for flexibility trading. The MCM operations scheme distinguishes five phases:

* Contract: AGR and DSO/TSO negotiate FlexOptions. This is flexibility that is reserved for DSO or TSO purposes and can
  be invoked by the DSO/TSO when needed. Typically, a contract includes availability remunerations and activation
  remunerations.
* Plan: information exchange between DSO/TSO and AGRs related to congestion points. This information exchange through
  the ‘Common Reference’ involves communication with the Common Reference Operator (CRO). The AGR carries out an initial
  portfolio optimization.
* Validate: the DSO uses D-prognoses to validate whether the demand and supply of energy can be distributed safely
  without any limitations. If congestion occurs, the DSO or TSO can procure flexibility from AGRs to resolve grid
  capacity issues.
* Operate: in the operate phase, the actual assets and appliances are dispatched and the AGR adheres to its D-prognoses.
  When required, DSOs/TSOs can invoke additional flexibility from AGRs to resolve unexpected congestion.
* Settle: in the settle phase, the flexibility that the AGR has sold to DSOs/TSOs is settled. For this purpose, the
  actual consumed and produced volumes are allocated to the responsible parties first. Any unresolved or disputed
  volumes are reconciled shortly afterwards.
