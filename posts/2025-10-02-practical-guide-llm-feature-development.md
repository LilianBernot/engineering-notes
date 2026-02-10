# A Practical Guide to Developing LLM-Powered Features

**Date:** 2025-10-02
**Tags:** [LLM, AI/ML, Development Practices, Testing, Optimization, Prompting]
**Status:** Published

---

## Introduction

Developing features powered by Large Language Models (LLMs) is fundamentally different from traditional software engineering. The non-deterministic nature of LLMs, combined with their sensitivity to prompt changes and token costs, requires a completely different development mindset.

This guide shares hard-earned lessons from building a Natural Language Query (NLQ) feature for a resource catalog, covering consistency challenges, testing strategies, and practical optimization techniques that apply to any LLM-powered application.

## The Reality: LLMs Are Not Deterministic

### The Inconsistency Problem

One of the first surprises when working with LLMs in production: **they're not consistent**, even with identical inputs.

During development of the Resource Catalog NLQ feature, I observed the same query producing completely different results in consecutive runs, seconds apart. Same input, same prompt, same settings—dramatically different outputs.

Here's what happened when running the identical query 4 times in a row:
- **Run 1**: Translated query correctly
- **Run 2**: Failed to parse, returned error
- **Run 3**: Translated with different filter structure
- **Run 4**: Different result than all previous attempts

The 5th attempt matched the 2nd result. This wasn't a one-time anomaly—it was the regular behavior.

### "Just Set Temperature to 0!"

Your first instinct might be: "Lower the temperature to 0 for deterministic output!"

Here's the problem: **temperature=0 doesn't mean deterministic**, despite what intuition suggests.

This is a common point of confusion documented across developer communities:
- Developers regularly report non-deterministic behavior at temperature=0
- OpenAI's forums have extensive threads on this topic
- The model still uses sampling techniques that introduce variability

In our case, temperature was already set to 0 from the beginning. It didn't solve the consistency problem.

In hindsight, this makes sense: if you wanted truly deterministic output for every input, you wouldn't need an LLM at all. You could write a deterministic program that would be faster, cheaper, and more controllable.

### Other Sources of Inconsistency

#### Day-to-Day Model Evolution

When you don't own the model, it evolves without your knowledge or control. Whether it's GPT, Claude, or other hosted LLMs, the models change constantly:

- Performance varies day-to-day
- Behavior shifts without version changes
- Users of tools like Cursor or Claude Code notice the model gets "dumber" some days

You're not hallucinating—many developers report this phenomenon. OpenAI community threads are full of discussions about ChatGPT degrading over time.

**Real example**: After spending an hour fighting with prompt engineering to improve suggestion quality, I gave up and stashed all changes. Out of curiosity, I reran tests with the original code. The results were suddenly much better than an hour earlier—**without any changes whatsoever**.

The model itself had shifted.

#### LLMs Are Chaotic Systems

In the mathematical sense, LLMs exhibit chaotic behavior:

> A chaotic function describes a deterministic system that appears random and unpredictable because of its extreme sensitivity to initial conditions—the "butterfly effect."

**In practice**: The smallest prompt change can have enormous impacts on output and consistency. A modification that seems trivial to humans—a reworded sentence, different punctuation, reordered instructions—can cause the LLM to drift completely off course.

This makes prompt engineering simultaneously powerful and frustrating. You're not working with a stable, linear system.

## Building and Testing LLM Applications

### The Testing Challenge

Given non-determinism, traditional unit testing approaches break down.

For the Resource Catalog NLQ feature, we needed to evaluate:
1. **Can the query be translated?** (binary: feasible/not feasible)
2. **Is the translation correct?** (complex: filter structure, operators, values)

The feasibility check is straightforward—it's binary. I could write unit tests to verify this attribute. It provided a good foundation for evaluating whether the LLM understood the request.

The filter correctness evaluation proved much more complex. I attempted various approaches:
- Weighted scoring for different filter components
- Structural comparisons
- Using another LLM to evaluate outputs (yes, LLMs reviewing LLMs)

Even with binary tests, **maintaining 100% success is impossible** given non-determinism. The chaotic nature makes it worse: improving one aspect often breaks another. This is expected and acceptable.

### Testing Strategies That Work

#### 1. Threshold-Based Tests

Instead of expecting perfection, use tolerance-based testing:

- Start with an achievable threshold (e.g., 70% success rate)
- Improve iteratively as you refine the system
- Increase thresholds as the application matures

For the feasibility attribute, I started at 70% success—already useful. Through development, this rose to 90% with more complex test cases. Progress felt tangible.

#### 2. Qualitative Assessment

**Trust your feelings**. Seriously.

When did you notice GPT getting "dumber"? You probably weren't running continuous benchmarks—you just felt something was wrong. Then you talked to others who felt the same way.

Apply this to development:
- Run manual tests regularly
- Notice when results feel worse
- Get other people to test the feature

**Confession**: Early on, I didn't dare ask teammates to test because I couldn't assess quality myself. Eventually, we ran bug bashes, and the feedback was invaluable. I should have done this sooner.

#### 3. Write Tests Anyway

Even if tests can't be perfect:
- They provide baselines
- They catch regressions
- They help you iterate
- They document expected behavior

Just accept that 100% pass rates aren't realistic.

### Development Best Practices

#### Take Your Time

LLMs are capricious. Sometimes poor results have nothing to do with your code—the model is just having a bad day.

When stuck:
- Wait a few hours
- Try again the next day
- Don't assume every problem is your fault

You can't perfectly control the model. Let it rest.

#### Be Incremental

I'm generally very incremental in development (small commits, step-by-step progress, frequent validation). For LLM work, this is **essential**.

Because LLMs are chaotic systems:
- Change one thing at a time
- Re-assess precision after each change
- Don't make multiple prompt modifications together

If you change three sentences in your prompt simultaneously, you won't know which one broke (or fixed) the behavior. Debug one variable at a time.

## Token Optimization: Making Your Prompts Efficient

### Why Tokens Matter

Tokens are the fundamental unit of LLM computation. More tokens mean:
- Higher API costs (OpenAI and others price by tokens)
- Slower response times
- Larger context windows required

Understanding tokenization: Try OpenAI's tokenizer tool to see how text maps to tokens. It's not always intuitive—punctuation, formatting, and structure all consume tokens.

**The Paradox**: You want prompts to be detailed and precise (more information = better results), but this costs more tokens. The solution is methodical prompt construction that maximizes information density.

### Key Optimization Goals

When building prompts, focus on:

1. **Reduce structural verbosity** (excessive line breaks, markdown headers)
2. **Condense similar rules** into compact patterns
3. **Reword verbose explanations** into examples
4. **Avoid unnecessary formatting** (bold, emoji) unless critical for comprehension

### Token Reduction Techniques

#### 1. Remove Unnecessary Line Breaks and Headers

Line breaks consume tokens. Each `\n\n` is a token.

You don't need extensive whitespace for the LLM to parse structure. Humans need visual separation; LLMs don't.

#### 2. Remove Emoji

Titles like `🚨 NEVER CREATE OR GUESS ATTRIBUTE NAMES 🚨` might feel emphatic, but emoji:
- Consume tokens
- Don't improve LLM comprehension in my testing

Save the emoji for user-facing content.

#### 3. Replace Repetitive Structures with Compact Patterns

**Verbose version** (118 tokens):
```
- For string, bool, json, and string[] attributes, you can only use:
    - is / is not
        - For strings: is environment production -> env:prod,
          is not environment production -> -env:prod
        - For booleans: is deprecated -> deprecation_time:*,
          is not deprecated -> -deprecation_time:*
        - For arrays: is in zone 1 -> zones:1,
          is not in zone 1 -> -zones:1
        - For json: is enabled -> enabled:true,
          is not enabled -> -enabled:true
```

**Compact version** (77 tokens):
```
For string, bool, JSON, and string[]: use `is` / `is not`:
- String: `env:prod`, `-env:prod`
- Bool: `deprecation_time:*`, `-deprecation_time:*`
- Array: `zones:1`, `-zones:1`
- JSON: `enabled:true`, `-enabled:true`
```

**Result**: 35% token reduction, identical comprehension.

#### 4. Simplify Logical Steps

**Verbose version**:
```
**First**: Check if standard attributes (env, region, team, service, etc.)
can satisfy the query
**Second**: Look at resource-specific attributes from getToolResourceTypeMetadata
**Third**: If neither standard nor resource-specific attributes work,
**ALWAYS call getToolResourceTypeTags**
```

**Optimized version**:
```
Check attributes in this order:
1. Standard attributes (env, region, etc.)
2. getToolResourceTypeMetadata
3. getToolResourceTypeTags (only if 1 & 2 fail)
```

Savings:
- Removed unnecessary bolding/capitalization
- Removed redundant tool explanations (LLM already has descriptions)
- Cleaner structure

### Token Optimization Techniques

#### 1. ALL-CAPS for Emphasis Over Bold

Counter-intuitively: `IMPORTANT` uses fewer tokens than `**IMPORTANT**` and LLMs parse it effectively for emphasis.

#### 2. Semicolon-Separated Lists

For listing items, use semicolons:
```
aws_account;aws_account_alias;region;account;name;aws;role
```

LLMs understand this format extremely well, and it's very token-efficient.

#### 3. JSON-Like Data Formats

For schema definitions, choose format based on complexity:

**Flat format** (most efficient):
```
aws_subnet_key:string;aws_vpc_key:string;block_device_mappings:json;boot_mode:string
```

✅ Very token-efficient
✅ Easy to parse
✅ Perfect for simple schemas
🚫 Not great for nested/complex types

**JSON format** (when you need structure):
```json
{
  "aws_subnet_key": "string",
  "aws_vpc_key": "string",
  "block_device_mappings": "json",
  "boot_mode": "string"
}
```

✅ Efficient and self-descriptive
✅ Easy to extend with metadata later
🚫 Slightly higher token count

Choose based on your needs. Don't use JSON when flat format suffices.

## Understanding Temperature

Since temperature comes up constantly in LLM development, here's a practical guide:

### What Is Temperature?

Think of temperature as a **creativity dial**:

**Low temperature (0 to 0.3)**:
- More deterministic behavior
- Chooses most likely, safe, common answers
- Best for: math, summarization, code generation, factual queries

**Medium temperature (0.5 to 0.7)**:
- Balance between predictability and creativity
- Best for: natural conversations, writing assistance, ideation

**High temperature (0.8 to 1.0+)**:
- More creative, random, risky outputs
- May generate unusual or surprising results
- Best for: brainstorming, storytelling, poetry, creative writing

### How It Works (Simplified)

Temperature affects word sampling during text generation:

**Analogy**: You're at a buffet of possible next words.
- Low temperature: mostly pick the most popular food
- High temperature: willing to try exotic or rare dishes

### Temperature Quick Reference

| Use Case | Suggested Temperature |
|----------|----------------------|
| Factual answers / Coding | 0.0 – 0.3 |
| Balanced conversation / General use | 0.5 – 0.7 |
| Creative writing / Brainstorming | 0.7 – 1.0+ |

**Remember**: Even at temperature=0, you won't get perfectly deterministic outputs. Adjust expectations accordingly.

## Conclusion: Embracing Uncertainty

Developing with LLMs requires a fundamental shift in engineering mindset:

**Accept non-determinism**: You can't eliminate it, only manage it. Build systems that are resilient to variation.

**Test differently**: Threshold-based testing, qualitative assessment, and iterative refinement replace traditional unit tests.

**Be incremental**: The chaotic nature of LLMs means small changes can have big impacts. Change one thing at a time.

**Optimize tokens**: Every token costs money and time. Ruthlessly optimize prompt structure while maintaining clarity.

**Take breaks**: Sometimes the model is just having a bad day. Don't fight it—wait it out.

The good news: despite all these challenges, LLM-powered features can deliver enormous value. The key is working with the technology's nature rather than against it. Build for flexibility, test pragmatically, and iterate constantly.

Your first version won't be perfect. Your tenth version won't be perfect either. And that's okay—welcome to the world of LLM engineering.

---

*Last updated: 2025-12-30*
