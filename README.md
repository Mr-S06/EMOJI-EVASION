# 🛡️ Emoji Evasion: Understanding and Defending Against Unicode-Based AI Attacks

> **The real battlefield isn't exotic math. It's text itself.**

A comprehensive resource for understanding how emojis, invisible Unicode characters, and hidden content can bypass AI safety systems — and how to defend against these attacks.

## 📋 Overview

Modern AI systems are vulnerable to surprisingly simple attack vectors that exploit how text is tokenized, parsed, and processed. This repository documents these techniques and provides practical defenses for engineers, security teams, and product managers protecting AI-powered systems.

### Why This Matters

- **Moderation Bypass**: Policy-violating content can evade automated filters
- **Prompt Injection**: Malicious instructions can be hidden in seemingly benign inputs
- **Data Exfiltration**: Hidden characters can cause models to reveal sensitive information
- **False Security**: What you see in logs may not be what the model processes

## 🎯 Key Threats

### 1. Emoji Smuggling
Inserting emojis at strategic positions to alter tokenization and bypass safety guardrails. Research has shown this technique achieving 100% success rates against certain guardrails under specific conditions.

### 2. Invisible Unicode Characters
Characters like zero-width spaces, zero-width joiners, and format controls that:
- Change how text is tokenized without visible changes
- Break substring matching in moderation rules
- Create mismatches between displayed and processed text

### 3. Hidden Content Channels
- **Image Metadata**: EXIF/IPTC/XMP fields carrying malicious instructions
- **URL Payloads**: Base64 or encoded commands in query strings
- **Hidden HTML**: Instructions in alt text, title attributes, or data-* fields

## 📚 Documentation

The complete technical documentation is available in this repository:

**[EMOJI SMUGGLING OR EMOJI EVASION.pdf](./EMOJI%20SMUGGLING%20OR%20EMOJI%20EVASION.pdf)**

This document covers:
- How Unicode and tokenization work under the hood
- Why seemingly trivial characters defeat safety checks
- Real-world impact on enterprise systems
- Comprehensive defense strategies and implementation guidance
- Monitoring and incident response plans

## 🔒 Defense Strategies

### Input Sanitization
```python
# Example: Basic Unicode normalization
import unicodedata

def sanitize_input(text):
    # Normalize to NFKC form
    text = unicodedata.normalize('NFKC', text)
    
    # Remove zero-width and invisible characters
    invisible_chars = [
        '\u200b',  # Zero-width space
        '\u200c',  # Zero-width non-joiner
        '\u200d',  # Zero-width joiner
        '\ufeff',  # Zero-width no-break space
    ]
    
    for char in invisible_chars:
        text = text.replace(char, '')
    
    return text
```

### Core Principles

1. **Consistent Normalization**: Apply the same Unicode normalization across your entire pipeline (UI, logging, moderation, model input)

2. **Treat All Inputs as Untrusted**: Images, links, HTML, and user text must be sanitized

3. **Strip Hidden Content**:
   - Remove or sanitize image metadata by default
   - Don't auto-fetch user-supplied URLs
   - Whitelist HTML tags and attributes, sanitize server-side

4. **Log Raw and Normalized**: Maintain visibility into what users send vs. what your system processes

5. **Monitor for Patterns**: Track spikes in suspicious Unicode usage, long metadata fields, or unusual tokenization

6. **Test Adversarially**: Regularly stress-test your models and filters with obfuscated inputs

## 🚀 Quick Start

### For Security Teams
1. Read the full documentation to understand attack vectors
2. Audit your current text processing pipeline for normalization gaps
3. Implement input sanitization at system boundaries
4. Set up monitoring for suspicious Unicode patterns

### For Engineers
1. Review your tokenization and preprocessing code
2. Ensure consistent Unicode normalization (recommend NFKC)
3. Add input validation that strips invisible characters
4. Test your moderation rules with emoji-obfuscated inputs

### For Product Managers
1. Understand the business risk (compliance, user trust, data leakage)
2. Prioritize defense implementation based on system exposure
3. Plan for ongoing adversarial testing
4. Establish incident response procedures

## ⚠️ Important Notes

- These attacks don't require sophisticated skills or model access
- They can be executed at scale against public-facing systems
- Patches alone are insufficient — defense requires systemic changes
- The threat landscape evolves constantly; continuous testing is essential

## 🤝 Contributing

Contributions are welcome! Please feel free to submit:
- Additional attack patterns and examples
- Defense code samples and libraries
- Case studies and incident reports (anonymized)
- Improvements to documentation

## 📖 Citation

If you use this research or documentation, please cite:


Sageer, S. (2025). Emoji Smuggling or Emoji Evasion. 
Technical Documentation, October 10, 2025.


## 📧 Contact

**Author**: Sufiyan Sageer  
**Date**: October 10, 2025


## 🔗 Additional Resources

- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [Unicode Security Considerations](https://unicode.org/reports/tr36/)
- [AI Security Best Practices](https://www.anthropic.com/index/claude-character-encoding)


**⚡ Remember**: In AI security, even minor vulnerabilities can escalate into major breaches. Act proactively — attackers are ready to exploit every small gap.

