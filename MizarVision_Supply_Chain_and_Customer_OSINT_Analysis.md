# MizarVision Supply Chain and Customer OSINT Analysis

**Assessment date:** September 23, 2026
**Entity:** Meentropy Technology (Hangzhou) Co., Ltd. / 觅熵科技（杭州）有限公司
**Trade name:** MizarVision
**Focus:** Supply-chain dependencies, imagery/data providers, technology partners, and identifiable customers

## Executive Assessment

MizarVision does not publicly disclose a comprehensive supplier or customer roster. Accordingly, this assessment only identifies suppliers, partners, customers, and dependencies where public evidence supports the relationship. Companies appearing only in commercial databases or industry lists without corroborating evidence are **not** treated as confirmed customers or suppliers.

MizarVision's most important external dependency is **remote-sensing data**. The company does not appear to operate its own satellite constellation. Its business model therefore depends on obtaining imagery and combining it with other information sources, including AIS maritime data, ADS-B aviation data, and other geospatial information.

The basic supply chain can be modeled as:

**Satellite operators / imagery aggregators → imagery/data acquisition partners → MizarVision ingestion infrastructure → AI/ML exploitation → AirSpace/AirView → API/report/intelligence product → customers**

This creates several identifiable supply-chain dependencies.

---

## 1. Beijing Sixiang Aishu — Satellite Imagery Partner

**Entity:** Beijing Sixiang Aishu Technology Co., Ltd. / 北京四象爱数科技有限公司
**Relationship:** Satellite imagery acquisition/application partner
**Confidence:** **HIGH**

The strongest publicly documented imagery-supply relationship identified is Beijing Sixiang Aishu Technology Co., Ltd., associated with the Sixiang/Four Squares commercial remote-sensing business.

On July 15, 2024, Sixiang and MizarVision announced a **strategic cooperation agreement covering the acquisition and application of high-value satellite imagery**.

The announcement indicates that the companies intended to combine their respective technology and resources in these areas.

This represents considerably stronger evidence of an upstream imagery relationship than merely identifying the satellite responsible for an image subsequently published by MizarVision.

**Source:**

https://www.taibo.cn/p/95844

---

## 2. Tianyi Research Institute / Spacety — SAR Data Relationship

**Entity:** Tianyi Research Institute / 天仪研究院 / Spacety
**Relationship:** SAR satellite-data commercialization partner
**Confidence:** **HIGH**

MizarVision and Tianyi Research Institute entered a strategic cooperation arrangement involving the **commercial application of SAR satellite-data products**.

The relationship included joint development and release of global forest-observation systems.

This potentially provides MizarVision access to an important second remote-sensing modality.

### Intelligence significance

Optical and SAR imagery provide complementary capabilities.

**Optical imagery** provides:

* intuitive high-resolution imagery;
* visual identification;
* object recognition; and
* conventional imagery interpretation.

**Synthetic Aperture Radar (SAR)** provides:

* day/night collection;
* substantially improved operation through clouds and weather;
* surface-change detection;
* detection of structures and certain physical characteristics; and
* collection when optical satellites are degraded by environmental conditions.

Access to both modalities is strategically significant for a GEOINT fusion platform.

The documented relationship does **not**, however, establish that all or even most MizarVision SAR imagery originates with Tianyi.

**Source:**

https://www.innospace.cn/news/detail/fe631653113de3673a637956bed1b554/2

---

## 3. Western Commercial Satellite Imagery

**Relationship:** Imagery source category
**Confidence:** **HIGH that Western imagery has appeared; direct procurement relationships remain unresolved**

Reporting following MizarVision's 2026 military-imagery activity identified imagery apparently originating from Western commercial remote-sensing systems.

Reported sources have included:

* Vantor / former Maxar Intelligence;
* Airbus; and
* historically, imagery associated with other Western commercial satellite operators.

However, identifying the satellite responsible for an image does **not** establish a direct commercial relationship between the satellite operator and MizarVision.

### Vantor / former Maxar

Imagery attribution involving Vantor/Maxar has been reported.

However, Vantor stated that it **does not sell imagery to Chinese entities**.

**Assessment:** Imagery origin may be identifiable, but a direct MizarVision procurement relationship is **NOT ESTABLISHED**.

### Planet Labs

Planet Labs stated that **MizarVision is not a customer**.

Planet also reportedly examined imagery published by MizarVision during the 2026 Iran conflict and stated that the examined imagery was not produced by Planet satellites.

**Assessment:** Planet Labs should **not** currently be identified as a confirmed MizarVision supplier.

### Airbus

Airbus-origin imagery has reportedly been identified among material associated with MizarVision.

However, no reliable evidence reviewed establishes that Airbus sells imagery directly to MizarVision.

**Assessment:** Possible originating imagery source; direct commercial relationship **UNPROVEN**.

**Source:**

https://www.dailyherald.com/20260404/nation-and-world/chinese-firms-market-iran-war-intelligence-exposing-u-s-forces/

---

## 4. Western Imagery Supply-Chain Intelligence Gap

The presence of Western-origin commercial imagery creates an important counter-intelligence question.

The key question is not simply:

> **Whose satellite collected the image?**

Instead, the important supply-chain question is:

> **Through what intermediary, reseller, exchange, archive, partner, or secondary commercial channel is MizarVision obtaining Western-origin imagery?**

The unresolved supply chain can therefore be modeled as:

**Western satellite operator → UNKNOWN INTERMEDIARY → UNKNOWN DISTRIBUTION CHANNEL → MizarVision → AI/GEOINT exploitation**

Possible mechanisms should **not** be assumed without evidence.

Potential collection requirements include identifying:

* authorized imagery resellers;
* regional imagery distributors;
* secondary commercial marketplaces;
* archive-access services;
* Chinese imagery aggregators;
* partner organizations possessing imagery licenses;
* API-based imagery services;
* subcontractors;
* research organizations with imagery access; and
* redistribution arrangements.

Determining this intermediary layer represents one of the highest-value outstanding supply-chain intelligence requirements concerning MizarVision.

---

## 5. Unidentified Commercial Satellite Startup

**Relationship:** MizarVision-described satellite partner
**Relationship confidence:** **HIGH**
**Identity confidence:** **UNRESOLVED**

MizarVision's June 28, 2026 Weibo reporting concerning imagery of Al Udeid Air Base in Qatar described the imagery source as a **medium-resolution satellite belonging to a MizarVision partner that is a startup company**.

The available evidence supports the existence of the relationship as described by MizarVision.

The available evidence reviewed does **not** reliably identify the startup.

It should therefore remain:

**UNKNOWN SATELLITE STARTUP — COLLECTION REQUIREMENT**

rather than attributing the relationship to a company based on circumstantial similarities.

**Source:**

https://weibo.com/p/1006066477763414/home?from=page_100606&mod=TAB

---

# 6. AIS Data Dependency

**Data type:** Automatic Identification System maritime tracking
**Dependency:** Confirmed platform/data requirement
**Provider:** **UNKNOWN**

MizarVision advertises the integration of AIS vessel information with satellite imagery and other sources.

AIS information can substantially enhance satellite-based maritime intelligence by associating imagery observations with:

* vessel identity;
* position;
* heading;
* speed;
* destination;
* historical movement; and
* behavioral patterns.

However, the underlying AIS provider used by MizarVision has not been reliably identified from the reviewed material.

The likely architecture is:

**AIS receivers → AIS aggregation network → data/API provider or reseller → MizarVision → multi-source fusion**

The specific commercial or government provider should **not** be inferred solely from similarities between MizarVision output and known AIS platforms.

**Assessment:**

**AIS dependency: HIGH confidence**
**AIS supplier: UNKNOWN**

---

# 7. ADS-B Data Dependency

**Data type:** Aircraft tracking / ADS-B
**Dependency:** Confirmed platform/data requirement
**Provider:** **UNKNOWN**

MizarVision also advertises integration of ADS-B aircraft information.

When fused with satellite imagery and other GEOINT, ADS-B can support analysis involving:

* aircraft identification;
* flight routes;
* airfield activity;
* tanker movements;
* reconnaissance aircraft;
* transport aircraft;
* deployment patterns; and
* changes in military aviation activity.

The underlying ADS-B data supplier has not been publicly identified in the reviewed evidence.

A likely generic supply chain is:

**ADS-B receiver network → aggregation service → API/data reseller → MizarVision → GEOINT fusion**

This architecture is presented as a generic model only and does **not** identify a particular supplier.

**Assessment:**

**ADS-B dependency: HIGH confidence**
**ADS-B supplier: UNKNOWN**

---

# 8. Chinese University of Hong Kong, Shenzhen

**Entity:** Chinese University of Hong Kong, Shenzhen / CUHK(SZ)
**Relationship:** Research and technology partner
**Confidence:** **HIGH**

MizarVision established the **CUHK(SZ)-MizarVision Joint Laboratory of Remote Sensing Technology** with the Chinese University of Hong Kong, Shenzhen.

The relationship includes joint research, graduate education, remote-sensing AI, and data-intelligence work.

This places CUHK-Shenzhen within MizarVision's broader **research and development ecosystem**.

It should **not** be characterized as a supplier or customer based on currently reviewed evidence.

**Classification:** Research/technology partner

**Source:**

https://www.innospace.cn/news/detail/9625aef7f12f836dc0a81b00e8280c6b/2

---

# 9. Xi'an Leitong

**Entity:** Xi'an Leitong
**Relationship:** Technology/integration partner
**Confidence:** **HIGH**

Chinese reporting identifies a strategic relationship between MizarVision and Xi'an Leitong involving:

* disaster monitoring;
* water engineering;
* agriculture and forestry;
* geological monitoring; and
* integrated space-ground geological-disaster monitoring radar.

The jointly developed system reportedly passed Ministry of Natural Resources expert review and was connected with the **China Geological Survey's Geological Cloud platform**.

This provides a documented connection between MizarVision's commercial technology ecosystem and a PRC government geological-information platform.

It does **not**, by itself, establish a relationship with the PLA or Chinese intelligence services.

**Classification:** Technology/integration partner

**Source:**

https://www.36kr.com/p/1531298067288065

---

# 10. Known Customer Categories

MizarVision publicly describes its users/customers as including:

* financial and futures organizations;
* news and media organizations;
* universities;
* research institutions;
* agriculture and forestry organizations;
* environmental organizations; and
* professional individual users.

MizarVision's website has also reported that AirSpace surpassed **500 registered users**.

Those users are not individually identified.

A registered user should also **not automatically be treated as a paying customer**.

**Sources:**

https://m.zhipin.com/companys/242fabc4cd11b2ae1HJ-09m1E1E~.html

https://www.mizarvision.com/

---

# 11. Phoenix Television Group — Confirmed Customer

**Entity:** Phoenix Television Group / 凤凰卫视集团
**Relationship:** Commercial services/data customer
**Confidence:** **HIGH**

Phoenix Television represents the clearest named MizarVision customer identified in the reviewed OSINT.

MizarVision reportedly signed a **commercial services contract with Phoenix Television Group** and became a **data provider to Phoenix**.

The arrangement reportedly involved providing Phoenix and affiliated organizations with:

* remote-sensing information;
* geographic-information technical support;
* space-air-ground integrated information; and
* defense situational-awareness information used in programming.

This evidence is stronger than a generic partnership announcement because the relationship is explicitly described as a **commercial services contract** and MizarVision as a **data provider**.

### Intelligence significance

The relationship demonstrates an identifiable downstream intelligence/information chain:

**Commercial satellite/sensor data → MizarVision ingestion → AI/GEOINT exploitation → defense situational-awareness product → Phoenix Television**

**Customer confidence: HIGH**

**Source:**

https://www.innospace.cn/news/detail/9625aef7f12f836dc0a81b00e8280c6b/2

---

# 12. South China Sea Strategic Situation Probing Initiative

**Entity:** South China Sea Strategic Situation Probing Initiative / SCSPI / 南海战略态势感知计划
**Relationship:** Contractual remote-sensing partner
**Confidence:** **HIGH for relationship; customer status unresolved**

Chinese reporting states that MizarVision reached a **remote-sensing application cooperation agreement** with SCSPI and that a contract was signed.

The relationship deserves particular counter-intelligence attention because SCSPI focuses extensively on military and security activity in the South China Sea.

However, the reviewed source describes the arrangement as **cooperation** and does not provide enough evidence to characterize SCSPI definitively as a conventional paying customer.

Therefore:

**Contractual relationship: CONFIRMED**

**Paying customer relationship: NOT ESTABLISHED**

This distinction should be preserved in intelligence reporting.

**Source:**

https://www.36kr.com/p/1531298067288065

---

# 13. Nanjing Forestry University

**Entity:** Nanjing Forestry University
**Relationship:** Remote-sensing/application partner
**Confidence:** **HIGH for partnership; customer status unresolved**

MizarVision publicly identifies cooperation with Nanjing Forestry University involving remote sensing applied to:

* biodiversity protection;
* rural ecological planning; and
* environmental protection.

MizarVision's corporate timeline also identifies Nanjing Forestry University in connection with the company's early AirSpace development/commercialization period.

There is insufficient evidence to conclude that Nanjing Forestry University is a paying MizarVision customer.

Therefore:

**Research/application partnership: CONFIRMED**

**Paying customer: UNCONFIRMED**

**Sources:**

https://www.36kr.com/p/1531298067288065

https://www.mizarvision.com/

---

# 14. Supply-Chain Intelligence Matrix

| Entity / Resource                   | Relationship                                                  | Confidence                                                                   |
| ----------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Beijing Sixiang Aishu               | High-value satellite imagery acquisition/application partner  | **HIGH**                                                                     |
| Tianyi Research Institute / Spacety | SAR satellite-data commercialization partner                  | **HIGH**                                                                     |
| Western commercial imagery          | Imagery source category                                       | **HIGH as category**                                                         |
| Vantor / former Maxar               | Western-origin imagery reportedly appearing downstream        | **Direct supply NOT established**                                            |
| Planet Labs                         | Historically alleged imagery source                           | **Direct supply NOT established; Planet says MizarVision is not a customer** |
| Airbus                              | Western imagery reportedly appearing downstream               | **Direct supplier UNKNOWN**                                                  |
| Unidentified satellite startup      | Medium-resolution imagery partner acknowledged by MizarVision | **HIGH relationship / UNKNOWN identity**                                     |
| AIS data                            | Platform intelligence input                                   | **HIGH dependency / supplier UNKNOWN**                                       |
| ADS-B data                          | Platform intelligence input                                   | **HIGH dependency / supplier UNKNOWN**                                       |
| CUHK Shenzhen                       | AI/remote-sensing R&D partner                                 | **HIGH**                                                                     |
| Xi'an Leitong                       | Monitoring/radar technology partner                           | **HIGH**                                                                     |
| Phoenix Television Group            | Commercial data/service customer                              | **HIGH**                                                                     |
| SCSPI                               | Contractual remote-sensing partner                            | **HIGH; customer status unresolved**                                         |
| Nanjing Forestry University         | Remote-sensing/application partner                            | **HIGH; customer status unresolved**                                         |

---

# 15. Counter-Intelligence Implications

The available evidence suggests that MizarVision should not be modeled simply as an isolated Chinese AI company consuming occasional commercial satellite imagery.

A better model is a **GEOINT integration and exploitation node within a larger commercial remote-sensing ecosystem**.

Its primary external dependency is consequently **continued access to imagery and other sensor/data feeds**, rather than ownership of space hardware.

Three supply chains deserve particular attention.

## Supply Chain A — Western Commercial Imagery

**Western satellite operator → UNKNOWN intermediary/distribution mechanism → MizarVision → AI/GEOINT exploitation**

The intermediary remains a major intelligence gap.

Current evidence does **not** establish that Western satellite operators knowingly sell directly to MizarVision, and some companies have explicitly denied such relationships.

---

## Supply Chain B — Chinese Remote-Sensing Ecosystem

A partially documented Chinese chain can be represented as:

**Chinese commercial satellite/SAR ecosystem → Sixiang/Tianyi/other partners → MizarVision → AI/GEOINT exploitation → Phoenix/SCSPI/other users**

Unlike the Western imagery chain, portions of this ecosystem are supported by publicly announced partnerships.

---

## Supply Chain C — Maritime and Aviation Tracking

Another significant dependency is:

**AIS/ADS-B collection networks → UNKNOWN aggregator/reseller → MizarVision → multi-**
