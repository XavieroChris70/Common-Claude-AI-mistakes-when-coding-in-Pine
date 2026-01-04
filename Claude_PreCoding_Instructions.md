# CLAUDE PRE-CODING INSTRUCTIONS

Upload this document before asking Claude to write code. These are documented recurring errors Claude makes and rules to prevent them.

---

## GENERAL RULES

1. **Read all uploaded source files FIRST before writing any code** - Do not make assumptions about logic or formulas
2. **Do NOT be creative** - Follow exact logic from provided files, do not add features not requested
3. **Do NOT hallucinate data** - Only use information explicitly provided in uploaded documents
4. **Verify code compiles** - Mentally execute through edge cases before delivering (empty arrays, bar 0, null values)
5. **No multi-line strings** - Keep all statements on single lines to avoid carriage return issues
6. **Declare variables at proper scope** - Variables used outside conditional blocks must be declared globally

---

## PINE SCRIPT SPECIFIC RULES

### Scope Requirements
- `plot()`, `plotshape()`, `bgcolor()`, `hline()`, `fill()`, `alertcondition()` MUST be at global scope, never inside `if` blocks
- Use ternary operators or include conditions in the series parameter for conditional plotting
- Variables accessed outside conditional blocks must be declared with `var` at global scope

### Array Safety
- ALWAYS check `array.size() > 0` before any array loop or access
- Loop syntax is `for i = 0 to array.size(arr) - 1`, not `range()`
- Use v5 syntax: `array.new<float>(0)` not `array.new(float, 0)` for v6

### Reserved Keywords - Do NOT Use as Variable Names
- `range`, `line`, `lines`, `label`, `box`, `table`
- `high`, `low`, `open`, `close`, `volume`, `time`, `bar_index`
- `color`, `location`, `style`, `size`
- Use prefixes/suffixes instead: `myRange`, `lineObj`, `priceHigh`

### Signal/Arrow Positioning (Non-Repainting)
- Use `barstate.isconfirmed` for all label and signal creation
- Set `offset=0` explicitly in `plotshape()` calls
- Use historical reference `[1]` correctly - signals should appear on the bar where pattern formed
- Labels must use `xloc=xloc.bar_index` to stick to candles

### Performance
- Declare tables with `var`, only update cell values - do not recreate every bar
- Implement max label/line limits and delete old objects
- Constrain lookback periods to available data (max 5000 bars)

### var Keyword Usage
- Use `var` when state MUST persist across bars
- Do NOT use `var` when value should recalculate each bar
- `var` variables use `:=` for reassignment, not `=`

### Type Safety
- Use `math.round()` when passing float to functions expecting int
- `color.new(color.white, 100)` for transparency, not `color.transparent`
- Unpack tuples from `request.security()` properly

---

## MT4/MT5 (MQL4/MQL5) SPECIFIC RULES

### Conversion Accuracy
- Use EXACT formulas from original indicator files
- Match default parameter values precisely
- Preserve original calculation order and logic
- Account for differences in array indexing (MQL uses `[0]` as current bar)

### Common Mistakes to Avoid
- Wrong period constants in `iMA()`, `iBands()`, `iATR()` calls
- Missing `extern` vs `input` parameter declaration differences between MQL4 and MQL5
- Incorrect buffer indexing for custom indicators
- Not handling `EMPTY_VALUE` or invalid data checks

---

## CODE CONVERSION RULES (Any Platform)

1. **Analyze source file completely** before writing any converted code
2. **Map all variables and functions** to target platform equivalents
3. **Preserve exact calculation logic** - do not "improve" or "optimize" formulas
4. **Match all default parameter values** from original
5. **Test edge cases mentally** - first bar, empty data, null values, division by zero
6. **Do not add features** not present in original unless explicitly requested

---

## BEFORE DELIVERING CODE - CHECKLIST

- [ ] All identifiers declared before use
- [ ] All plotting functions at global scope
- [ ] Array bounds checked before access
- [ ] No reserved keywords as variable names
- [ ] No carriage returns or multi-line strings
- [ ] `barstate.isconfirmed` used for labels/signals
- [ ] `var` used correctly (only for persistent state)
- [ ] Tables declared with `var`, updated not recreated
- [ ] Edge cases handled (empty arrays, bar 0, null values)
- [ ] Original source file logic followed exactly

---

## HOW TO REPORT ERRORS TO CLAUDE

Use these exact phrases for faster fixes:

- "Undeclared identifier error on [variable]" → Scope issue
- "Cannot use [function] in local scope" → Move to global scope
- "Signals are repainting / moving" → Add `barstate.isconfirmed`
- "Array index out of bounds" → Add size check before loop
- "Code doesn't match original" → Re-read source file, follow exactly
- "Remove carriage returns" → Line break cleanup needed
- "You were creative" → Revert to only what was in source files

---

## EXPECTED BEHAVIOR

When these instructions are followed, Claude should:

1. Read and analyze all uploaded files before writing code
2. Produce code that compiles on first attempt
3. Follow exact logic from source files without deviation
4. Handle edge cases properly
5. Use correct scope for all functions and variables
6. Deliver non-repainting signals that stick to bars
7. Not add unrequested features or "improvements"

If Claude violates these rules, reference this document and the specific rule number.

---

*Document Version: 1.0*
*Based on: Documented error analysis from extensive coding sessions*
