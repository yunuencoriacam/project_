You are extracting and NORMALIZING pricing data from an MSA contract into a standardized fee table.

Your goal is to map the contract into a fixed structure used internally.

---

## OUTPUT FORMAT (STRICT)

Return ONLY one CSV row.

Columns EXACTLY:

Client,Inbound Tier I,USDT/C→USD I,Inbound Tier II,USDT/C→USD II,Inbound Tier III,USDT/C→USD III,KYB,KYC,Monthly Biz,Monthly Consumer,FedNow,ACH Standard,ACH Same Day,Wire Domestic,Wire International,Outbound I,Outbound II,Outbound III, ACH Standard, ACH Same day, Wire Domestic, Wire International

---

## WHAT TO EXTRACT

### INBOUND (Fiat → Crypto)

For each Tier (I, II, III):

1. Inbound fee
   - Look for: bank access, inbound fee, processing fee
2. FX fee (USDT/C → USD)

---

### OUTBOUND (Crypto → Fiat)

For each Tier:

- USD → USDT/C (whichever represents off-ramp)
- If only one value exists → apply to all tiers

---

### FIXED FEES

- KYB (one-time)
- KYC (one-time)
- Monthly business account fee
- Monthly consumer account fee

---

### PAYMENT RAILS

- FedNow
- ACH Standard
- ACH Same Day
- Wire Domestic
- Wire International

---

## NORMALIZATION RULES (CRITICAL)

1. Waived fees:
   - If marked "waived" → use 0 or 0.00%

2. Replaced / crossed-out values:
   - Example: 0.12% → 0.03%
   - ALWAYS use the final value (0.03%)

3. Missing values:
   - Leave empty

4. Blended fees:
   - If only one % is given (no split between inbound and FX):
     → use the same value for both

5. Identical tiers:
   - If the same value applies to all tiers → repeat it

6. Fee mapping logic:
   - Bank access → Inbound fee
   - On-ramp → FX fee
   - Off-ramp → Outbound

7. Format:
   - Percentages: 0.08%
   - USD: $15 (no commas)

---

## OUTPUT RULES

- Return ONLY one CSV row
- No explanation
- No extra text
- No headers
- No code block

---

## EXAMPLE

Graph,0.08%,0.08%,0.08%,0.06%,0.04%,0.05%,$10,$2,$10,$2,$0.60,$0.40,$0.75,$15,$30,0.08%,0.06%,0.04%, $0.40,$0.75,$0.60,$15.0,$30.0

---

Now extract from this contract:

Client: Graph

Exhibit C Platform fees
Virtual Accounts
Tier I II III
Monthly payment volume $0 - $15M $15M - $30M $30M+

Virtual USD account

Virtual account 0.08% 0.06% 0.04%

Bank pass thru fees Business customers Consumer customers
Onboarding KYC/KYB (1 time) $10 $2
Monthly account fees $5 $2
Fednow (instant payments) $0.60
ACH Same day: $0.75 Standard: $0.40
Wires Domestic: $15 International: $30
Payouts (Offramp) & Payins (Onramp)
ON RAMP
Pricing Tiers I II III
Monthly payment volume $0 - $15M $15M - $30M $30M+
USD ➔ USDT/C 0.16% 0.08% 0.06% 0.04%
COP ➔ USDT/C 0.10% 0.07% 0.05%
MXN ➔ USDT/C 0.30% 0.10% 0.10%
OFF RAMP
USDT/C ➔ USD 0.18% 0.08% 0.13% 0.06% 0.05%
USDT/C ➔ COP 0.40% 0.35% 0.27%
USDT/C ➔ MXN 0.25% 0.20% 0.12%
USDT/C ➔ USD *Hong Kong 0.50% 0.45% 0.35%
USDT/C ➔ USD *China 0.50% 0.45% 0.35%


