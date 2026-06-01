# student-ctd-basket-repo-shock-roll

A simple educational fixed income project exploring how repo financing shocks can alter relative Cheapest-To-Deliver (CTD) basket dynamics in Treasury futures.

This project models:

* accrued interest
* dirty price
* fair value futures pricing
* invoice pricing
* net basis
* repo shock sensitivity across a 12-bond CTD basket

The goal is not precision production pricing.

The goal is transparent conceptual understanding of:

* financing
* carry
* invoice mechanics
* basis behavior
* CTD persistence vs CTD roll dynamics

---

# Core Idea

Repo financing changes futures fair value.

Changes in fair value alter:

* invoice price
* net basis
* relative basket cheapness

This project explores whether a repo shock can alter CTD ranking across a Treasury futures basket.

---

# Inputs

Each bond in the basket contains:

| Input | Description            |
| ----- | ---------------------- |
| CF    | Conversion factor      |
| Par   | Bond par value         |
| Clean | Clean price            |
| Rate  | Coupon rate (decimal)  |
| Freq  | Coupon frequency       |
| Since | Days since last coupon |
| In    | Days in coupon period  |

Global inputs:

| Input | Description              |
| ----- | ------------------------ |
| DTD   | Days to futures delivery |
| Repo  | Base repo financing rate |
| Shock | Repo shock change        |

Important:

```text
Shocked Repo = Repo + Shock
```

The shock is a CHANGE to repo, not the final repo itself.

---

# Outputs

| Output           | Description                               |
| ---------------- | ----------------------------------------- |
| Acc Int          | Current accrued interest                  |
| Dirty            | Dirty bond price                          |
| Futures FV Repo  | Fair value futures price at repo          |
| Net Basis Repo   | Net basis at repo                         |
| Futures FV Shock | Fair value futures price after repo shock |
| Net Basis Shock  | Net basis after repo shock                |

The lowest net basis represents the CTD candidate.

---

# Formulas

## Coupon Payment

```text
Coupon Payment = (Coupon Rate × Par) / Frequency
```

---

## Accrued Interest

```text
Accrued Interest =
(Days Since Coupon / Days In Coupon Period)
× Coupon Payment
```

---

## Dirty Price

```text
Dirty Price =
Clean Price + Accrued Interest
```

---

## Fair Value Futures

```text
Fair Value Futures =
(
Dirty
+
(Dirty × Repo × (DTD / 360))
-
(
Accrued Interest
+
(
DTD / Days In Coupon Period
× Coupon Payment
)
)
)
/ Conversion Factor
```

---

## Invoice Price

```text
Invoice =
(Futures × Conversion Factor)
+
(
Accrued Interest
+
(
DTD / Days In Coupon Period
× Coupon Payment
)
)
```

---

## Net Basis

```text
Net Basis =
Dirty Price - Invoice
```

More negative net basis:
→ cheaper-to-deliver candidate.

---

# Repo Shock Logic

Shock scenario:

```text
Shocked Repo = Repo + Shock
```

The model recalculates:

* futures fair value
* invoice price
* net basis

for the shocked financing environment.

This allows observation of:

* CTD persistence
* CTD instability
* basket repricing under financing stress

---

# Important Notes

This is an educational framework.

It is intentionally transparent and simplified.

The model does NOT include:

* market futures pricing
* yield curve shifts
* convexity adjustments
* implied repo calculations
* delivery options
* special repo conditions
* funding fragmentation
* market liquidity effects
* actual CME delivery mechanics
* term structure interpolation

This project should not be used for trading or valuation purposes.

---

# Main Educational Insight

This project demonstrates that Treasury futures pricing is fundamentally connected to:

```text
cash bonds
+
financing
+
carry
+
invoice mechanics
```

CTD behavior is not static.

Financing conditions can alter relative basket cheapness through changes in:

* carry
* fair value futures
* invoice pricing
* net basis relationships

---

# Project Goal

Simple.
Transparent.
Mechanically understandable.

The purpose of this project is to make Treasury futures financing and CTD behavior easier to visualize and study.
