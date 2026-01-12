# Prompt Engineering Strategy

**Project:** GenAI Email & Report Drafting System  
**Last Updated:** 2026-01-09  
**Status:** Implemented

---

## Overview

The GenAI Email & Report Drafting System uses **instruction-based prompt engineering**, optimized for **Google Gemini's instruction-following capabilities**. The system employs structured prompts that guide the AI to generate professional, contextually appropriate content for business and academic environments.

---

## Prompt Engineering Approach

### Instruction-Based Prompting

The system uses **instruction-based prompt engineering** rather than few-shot examples or template-based approaches. This approach:

- **Leverages Gemini's Strengths:** Google Gemini excels at following explicit instructions
- **Maintains Flexibility:** Allows dynamic content generation based on user input
- **Ensures Consistency:** Structured prompts produce consistent output quality
- **Reduces Complexity:** Simpler than maintaining example libraries

### Core Principles

1. **Role Definition:** Clear AI assistant role specification
2. **Explicit Task Description:** Unambiguous task framing
3. **Tone and Format Constraints:** Explicit style and structure guidance
4. **Context Integration:** User input seamlessly integrated into prompts
5. **Output Formatting:** Clear instructions for desired output format

---

## Prompt Components

### 1. Role Definition

Every prompt begins with a clear role definition:

```text
You are an expert professional communication assistant specializing in
corporate and academic writing. Your task is to generate clear, well-structured,
and contextually appropriate content for business and academic environments.
```

**Purpose:**

- Sets the AI's persona and expertise level
- Establishes context for the generation task
- Guides the AI's writing style and approach

### 2. Task Description

Explicit task framing for the specific content type:

**For Emails:**

```text
## Task
Generate a professional email based on the provided context, recipient,
and subject. The email should be clear, concise, and appropriate for the
specified tone and business context.
```

**For Reports:**

```text
## Task
Generate a comprehensive report based on the provided topic and requirements.
The report should be well-organized, informative, and suitable for professional
presentation.
```

### 3. Tone Constraints

Explicit tone instructions guide the writing style:

**Available Tones:**

- **Professional** - Professional and respectful tone, suitable for most business communications
- **Casual** - Casual and friendly while maintaining professionalism
- **Formal** - Formal and reserved with proper business etiquette
- **Friendly** - Warm and friendly while remaining professional

**Tone Guidance Example:**

```text
## Tone
Maintain a professional tone throughout. Use clear, direct language.
Avoid overly casual expressions while remaining approachable.
```

### 4. Format and Structure Constraints

**For Emails:**

- Email body only (subject line handled separately)
- Appropriate greeting and closing
- Clear paragraph structure
- Professional formatting

**For Reports:**

- Structure types: `executive_summary`, `detailed`, `bullet_points`
- Section organization
- Appropriate depth and detail level
- Professional presentation format

### 5. Context Integration

User-provided context is integrated into the prompt:

```text
## Context
{user_provided_context}

## Recipient
{recipient_name}

## Subject
{email_subject}
```

---

## Prompt Structure Examples

### Email Generation Prompt

**Complete Prompt Structure:**

```text
You are an expert professional communication assistant specializing in
corporate and academic writing. Your task is to generate clear, well-structured,
and contextually appropriate content for business and academic environments.

## Task
Generate a professional email based on the provided context, recipient,
and subject. The email should be clear, concise, and appropriate for the
specified tone and business context.

## Context
{user_provided_context}

## Recipient
{recipient_name}

## Subject
{email_subject}

## Tone
Maintain a {tone} tone throughout. Use clear, direct language.
Avoid overly casual expressions while remaining approachable.

## Output
Generate the email content now. Include only the email body without subject line.
```

### Report Generation Prompt

**Complete Prompt Structure:**

```text
You are an expert professional communication assistant specializing in
corporate and academic writing. Your task is to generate clear, well-structured,
and contextually appropriate content for business and academic environments.

## Task
Generate a comprehensive report based on the provided topic and requirements.
The report should be well-organized, informative, and suitable for professional
presentation.

## Topic
{report_topic}

## Key Points
{key_points_to_include}

## Tone
Maintain a {tone} tone throughout. Use professional language appropriate
for business or academic contexts.

## Structure
Generate a {structure_type} report with appropriate sections and depth.

## Output
Generate the complete report now with proper formatting and structure.
```

---

## Implementation Details

### Backend Implementation

**Module:** `backend/services/prompt_engine.py`

**Key Classes:**

- `PromptEngine` - Main prompt construction class
- `ToneType` - Enumeration of valid tone types
- `ReportStructureType` - Enumeration of valid report structures

**Key Methods:**

- `build_email_prompt()` - Constructs email generation prompts
- `build_report_prompt()` - Constructs report generation prompts
- `validate_input()` - Validates and sanitizes user input
- `validate_tone()` - Validates tone parameter
- `validate_structure()` - Validates report structure parameter

**Usage Example:**

```python
from services.prompt_engine import PromptEngine

# Generate email prompt
email_prompt = PromptEngine.build_email_prompt(
    context="Request a meeting to discuss Q1 project milestones",
    recipient="Engineering Team",
    subject="Q1 Milestone Review Meeting",
    tone="professional"
)

# Generate report prompt
report_prompt = PromptEngine.build_report_prompt(
    topic="Security Vulnerability Assessment",
    key_points="Focus on web application vulnerabilities and API authentication",
    tone="formal",
    structure="detailed"
)
```

### Input Validation

**Validation Rules:**

- **Context/Topic:** Required, non-empty, max 5000 characters
- **Tone:** Must be one of: `professional`, `casual`, `formal`, `friendly`
- **Structure (Reports):** Must be one of: `executive_summary`, `detailed`, `bullet_points`
- **Recipient/Subject:** Optional, max 500 characters each
- **Key Points:** Optional, max 5000 characters

**Error Handling:**

- Empty inputs raise `ValueError` with descriptive messages
- Invalid tone/structure values raise `ValueError` with valid options listed
- Length violations raise `ValueError` with maximum length specified

---

## Tone Selection Guide

### Professional (Default)

**Best For:**

- General business communications
- Internal team communications
- Standard client interactions
- Most corporate emails

**Characteristics:**

- Clear and direct
- Respectful and courteous
- Balanced formality
- Approachable but professional

### Formal

**Best For:**

- Executive communications
- Official announcements
- External stakeholder communications
- Academic reports
- Legal or compliance documents

**Characteristics:**

- Reserved and respectful
- Proper business etiquette
- Structured and formal language
- Minimal casual expressions

### Casual

**Best For:**

- Internal team communications
- Informal updates
- Collaborative communications
- Non-critical messages

**Characteristics:**

- Friendly and approachable
- Conversational tone
- Maintains professionalism
- Less rigid structure

### Friendly

**Best For:**

- Relationship-building communications
- Thank you messages
- Positive feedback
- Welcoming messages

**Characteristics:**

- Warm and personable
- Positive and encouraging
- Professional yet warm
- Engaging tone

---

## Report Structure Types

### Executive Summary

**Purpose:** High-level overview for decision-makers

**Characteristics:**

- Concise (500-800 words)
- Key findings and recommendations
- Strategic focus
- Minimal technical detail

**Best For:**

- C-level presentations
- Board reports
- Quick decision-making
- High-level overviews

### Detailed

**Purpose:** Comprehensive analysis with full context

**Characteristics:**

- Comprehensive (1000-1500 words)
- Multiple sections
- In-depth analysis
- Full context and background

**Best For:**

- Technical documentation
- Research reports
- Comprehensive analysis
- Full project documentation

### Bullet Points

**Purpose:** Scannable, organized format

**Characteristics:**

- Organized bullet format
- Easy to scan
- Key points highlighted
- Quick reference format

**Best For:**

- Action items
- Summary documents
- Quick reference guides
- Status updates

---

## Prompt Optimization Strategies

### 1. Clear Instructions

- Use explicit, unambiguous language
- Avoid vague or ambiguous terms
- Specify exact output requirements

### 2. Structured Format

- Use markdown-style headers for organization
- Separate different prompt components clearly
- Use consistent formatting throughout

### 3. Context Integration

- Seamlessly integrate user input
- Maintain context throughout the prompt
- Provide sufficient background information

### 4. Output Formatting

- Specify exact output format requirements
- Clarify what to include/exclude
- Provide formatting examples when needed

### 5. Tone Consistency

- Explicit tone instructions
- Examples of tone-appropriate language
- Clear boundaries for tone variation

---

## Integration with Google Gemini

### API Integration

Prompts are sent to Google Gemini API via `GeminiService`:

```python
from services.gemini_service import GeminiService
from services.prompt_engine import PromptEngine

# Build prompt
prompt = PromptEngine.build_email_prompt(
    context=context,
    recipient=recipient,
    subject=subject,
    tone=tone
)

# Generate content
gemini_service = GeminiService()
result = gemini_service.generate_content(
    prompt=prompt,
    correlation_id=request_id
)
generated_content = result['content']
```

### Gemini Configuration

**Model:** Google Gemini (configured via `GEMINI_API_KEY`)

**Parameters:**

- Temperature: Configurable (default: 0.7)
- Max tokens: Handled by Gemini API
- Timeout: 30 seconds (configurable)

---

## Best Practices

### 1. Input Validation

- Always validate user input before prompt construction
- Enforce length limits to prevent API abuse
- Validate tone and structure parameters

### 2. Error Handling

- Handle invalid inputs gracefully
- Provide clear error messages
- Log prompt construction errors for debugging

### 3. Security

- Sanitize user input to prevent prompt injection
- Validate all parameters before use
- Limit input length to prevent resource exhaustion

### 4. Consistency

- Use consistent prompt structure across all content types
- Maintain role definition consistency
- Follow established formatting patterns

### 5. Testing

- Test prompts with various inputs
- Validate output quality
- Monitor prompt effectiveness

---

## Testing

### Prompt Engine Tests (`backend/tests/test_prompt_engine.py`)

- ✅ Email prompt construction
- ✅ Report prompt construction
- ✅ Input validation
- ✅ Tone validation
- ✅ Structure validation
- ✅ Error handling
- ✅ Edge cases

**Run Tests:**

```powershell
cd backend
pytest tests/test_prompt_engine.py -v
```

---

## Related Documentation

- [Repository Structure](07_repository_structure.md) - Project structure
- [Technical Documentation](04_technical.md) - Technical implementation
- [Requirements Specification](02_requirements.md) - System requirements
- [Architecture Plan](05_architecture_plan.md) - System architecture

---

**Last Updated:** 2026-01-09  
**Maintained By:** Viswanatha Swamy P K
