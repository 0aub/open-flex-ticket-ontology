# Open Flex Ticket Ontology 🚌🚆  
_Re-implementation & Extension of the Belgian Railways knowledge-based pricing engine_

---

## 1  Project Brief

The 2024 RuleML+RR companion paper **“Ontologies and Semantic Rules in Real Life”** describes how Belgian Railways (B-Rail) replaced a legacy product-pricing system with an **ontology-centric** stack (OWL + textual-SWRL).  
Key claims in the paper:

* end-to-end throughput **> 160 requests s⁻¹** (95 th pct = 75 ms)  
* full business logic captured declaratively  
* zero production defects since launch

This repository **re-creates** that approach with _public, lightweight tools_ and demonstrates how easily new pricing rules can be added.

---

## 2  What’s Implemented 🔧

| Layer | Our implementation |
|-------|--------------------|
| **Ontology** | 7 classes (`Person`, `Student`, `Employer`, `Ticket`, `FlexibleSeasonTicket`, `Warning`, `TravelAboPrice`) + 8 properties |
| **Rule engine** | 200-line `TextRuleEngine` that understands the paper’s textual-SWRL syntax (aggregates, NAF, existential “function of”) |
| **Core rules from paper** | `age-of-client` • `travel-pass-have-prices` • `zone-and-places-…` |
| **Extra business rules** | * Student – 20 % discount  
* Peak +15 %  
* Multi-modal + €25 |
| **Data generator** | 10 employers • 500 travellers • distributions: 15 % students, 35 % peak, 25 % multimodal |
| **Plots & metrics** | KPI bar-chart, latency histogram, employer-contribution warnings |
| **Performance** | 5 000 ontology look-ups → **2.2 µs mean** / **458 k qps** on single Colab core |

Everything lives in a single Jupyter/Colab notebook (`Re_Implementation_and_Extension_...ipynb`) and two helper images in `figs/`.

---

## 3  Quick Start 🚀

```bash
# clone and open the notebook (Colab or Jupyter)
git clone https://github.com/<you>/open-flex-ticket-ontology.git
```

1. Run the notebook top-to-bottom.  
2. Generated files appear in `figs/` and `rail_demo_with_text_rules.owl`.

_No external datasets or credentials required._

---

## 4  Repository Structure

```
.
├── Re_Implementation_and_Extension_of_the_Belgian_Railways_...ipynb
├── figs/
│   ├── kpi_student.png
│   └── latency_hist.png
├── rail_demo_with_text_rules.owl
├── open_flex_ticket_ontology_slides.pptx
├── report.tex
└── README.md
```

---

## 5  Results 📊

| Metric | Value |
|--------|-------|
| Mean latency | **2.2 µs** |
| 95 th percentile | **2.6 µs** |
| Single-process throughput | **458 962 queries s⁻¹** |
| Avg employee pay (non-student) | €105.9 |
| Avg employee pay (student) | **€70.5** |

12 `Warning` individuals flag employers whose contribution < 30 %.

---

## 6  How to Add a New Rule 📝

Open `rule_texts.txt` (or the cell in the notebook) and append:

```text
rule senior-discount
if ?p is a Senior
   and ?p has_ticket ?t
then ?t price ?newPrice
   where ?newPrice := 0.7 * ?t.price .
```

Run `engine.run()` once—no Python code edits, no DB migrations.

---

## 7  Limitations & Next Steps

* Synthetic dataset only ⇒ import real ticket logs.  
* Single-thread materialisation ⇒ evaluate multi-core reasoners.  
* Replace SQLite with a production graph DB for persistence & concurrency.

---

## 8  Citation

```
M. Vanden Bossche, L. Guizol, R. Le Brouster,  
"Ontologies and Semantic Rules in Real Life", RuleML+RR Companion, 2024.
```

---

## 9  License

MIT — free for academic or commercial use.  
Images copied from the RuleML paper remain © their authors.

---

*Happy reasoning!* 🤖
