# a11y-agent

A specialized Claude Code subagent for comprehensive WCAG AA web accessibility validation and remediation guidance.

## Purpose

The **a11y-agent** is an expert accessibility validator that helps developers build inclusive web applications by:

- 🔍 **Scanning** live websites and local code for WCAG AA compliance violations
- 📊 **Analyzing** accessibility issues with detailed explanations of user impact
- 🛠️ **Providing** framework-specific fix suggestions (React, Angular, Vue, vanilla HTML)
- 📚 **Educating** developers on WCAG 2.1/2.2 Level AA requirements
- ✅ **Verifying** accessibility improvements

The agent uses **axe-core** (the industry-standard accessibility testing engine) combined with Claude's expertise to deliver actionable, detailed accessibility audits.

## Key Features

- **Automated Scanning**: Uses Puppeteer for live sites, filesystem tools for local code
- **WCAG AA Compliance**: Focuses on Level AA success criteria (most common legal requirement)
- **Framework-Aware**: Provides fixes tailored to React, Angular, Vue, or vanilla HTML/JS
- **Detailed Impact Analysis**: Explains how violations affect users with disabilities
- **Before/After Code Examples**: Shows problematic code and corrected versions
- **Severity-Based Reporting**: Prioritizes violations by impact (Critical → Minor)
- **Accessible Output**: Uses text labels and semantic icons (not color-dependent)

## Installation

### 1. Create the agents directory

**macOS/Linux:**
```bash
mkdir -p ~/.claude/agents
```

**Windows (via WSL):**
```bash
mkdir -p ~/.claude/agents
```

### 2. Save the agent file

Copy the `a11y-agent.md` file to your agents directory:

```bash
# User-level (available in all projects)
cp a11y-agent.md ~/.claude/agents/

# OR project-level (specific to one project)
mkdir -p .claude/agents
cp a11y-agent.md .claude/agents/
```

### 3. Verify installation

```bash
# List available agents
claude agents list

# You should see "a11y-agent" in the list
```

## Usage

### Invoke the agent

Start Claude Code and mention the agent:

```bash
claude
```

Then in the conversation:
```
@a11y-agent scan https://example.com
```

Or start directly:
```bash
claude --subagent a11y-agent
```

### Example workflows

#### Scan a live website
```
@a11y-agent

What would you like me to validate for WCAG AA compliance?
1. Live website - full site scan (provide base URL)
2. Live website - specific page (provide URL)
3. Local code - file or directory (provide path)

You: Option 2 - https://mywebsite.com/products

[Agent scans the page and provides detailed report]
```

#### Scan local components
```
@a11y-agent validate ./src/components/Header.jsx

[Agent analyzes the file and reports violations]
```

#### Full directory scan
```
@a11y-agent scan all components in ./src/components/

[Agent finds and scans all relevant files]
```

## Report Format

The agent provides structured, accessible reports:

```
🔍 Accessibility Scan Results - WCAG AA Compliance

Scan Target: https://example.com
Framework: React

Summary:
- Total violations: 8
- ❗ CRITICAL: 2
- ⚠️  SERIOUS: 3
- ℹ️  MODERATE: 2
- • MINOR: 1
- ✓ PASSES: 45

---

### [1] color-contrast - ❗ CRITICAL

WCAG Criterion: 1.4.3 Contrast (Minimum) - Level AA

Issue Description:
Text color does not meet minimum contrast ratio requirements

Element(s) Affected:
```html
<p class="subtitle">Welcome to our site</p>
```

Location: .subtitle (multiple instances)

Impact on Users:
- Users with visual impairments: Cannot read low-contrast text
- Users with color blindness: Reduced readability
- Users in bright environments: Text becomes illegible

How to Fix:

Current Code (Problematic):
```css
.subtitle {
  color: #999;
  background: #fff;
}
```

Fixed Code:
```css
.subtitle {
  color: #767676; /* 4.5:1 contrast ratio */
  background: #fff;
}
```

Explanation:
WCAG AA requires a minimum contrast ratio of 4.5:1 for normal text.
The original #999 gray on white provides only 2.8:1 contrast.
The fixed color #767676 provides 4.54:1, meeting the requirement.

Resources:
- WCAG Reference: https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum
---

[Additional violations...]
```

## Severity Indicators

The agent uses accessible, non-color-dependent indicators:

- **❗ CRITICAL** - Blocks accessibility, immediate fix required
- **⚠️ SERIOUS** - Major barriers for users with disabilities
- **ℹ️ MODERATE** - Significant issues to address
- **• MINOR** - Improvements recommended
- **✓ PASS** - Meets WCAG AA requirements

## WCAG AA Coverage

The agent checks all WCAG 2.1 Level A and AA success criteria:

**Perceivable**
- Text alternatives (1.1.1)
- Color contrast (1.4.3, 1.4.5)
- Reflow and text spacing (1.4.10, 1.4.12)

**Operable**
- Keyboard accessibility (2.1.1, 2.1.2)
- Focus order and visibility (2.4.3, 2.4.7)
- Pointer gestures (2.5.1-2.5.4)

**Understandable**
- Language identification (3.1.1, 3.1.2)
- Predictable behavior (3.2.1-3.2.4)
- Error handling (3.3.1-3.3.4)

**Robust**
- Valid markup (4.1.1)
- Name, role, value (4.1.2, 4.1.3)

## Requirements

- **Claude Code** installed and configured
- **Puppeteer** tool enabled (for live website scanning)
- **Filesystem** tools enabled (for local code scanning)

## Important Notes

⚠️ **Automated testing limitations**: Automated tools like axe-core catch approximately 30-40% of accessibility issues. The agent will remind you to also perform:

- Manual testing with screen readers (NVDA, JAWS, VoiceOver)
- Keyboard-only navigation testing
- Testing with browser zoom and text resizing
- Real user testing with people who have disabilities

🎯 **Framework support**: The agent provides framework-specific fixes for:
- React (JSX syntax, hooks, accessible component libraries)
- Angular (template syntax, CDK accessibility utilities)
- Vue (template directives, Composition API)
- Vanilla HTML/CSS/JavaScript

## Troubleshooting

**Agent not found:**
- Verify the file is in `~/.claude/agents/a11y-agent.md`
- Check that the filename matches the `name:` field in the YAML frontmatter
- Restart Claude Code

**Scan fails on live site:**
- Ensure Puppeteer tools are enabled
- Check that the URL is accessible
- Verify you have internet connectivity

**No violations found but site has issues:**
- Remember automated tools catch only ~30-40% of issues
- Use the agent to request manual testing guidance
- Consider running scans on multiple pages

## Contributing

Found a way to improve the agent? Suggestions for additional features:

- Additional WCAG levels (A, AAA)
- Export reports in different formats (JSON, CSV, PDF)
- Integration with CI/CD pipelines
- Custom rule configurations
- Automated fix application

## Resources

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [axe-core Documentation](https://github.com/dequelabs/axe-core)
- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

## License

This agent definition is provided as-is for use with Claude Code.

---

**Made with ♿ for accessible web development**