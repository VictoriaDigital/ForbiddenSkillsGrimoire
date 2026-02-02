# 🔮 ForbiddenSkillsGrimoire

*A collection of agent skills for edge cases and alternative approaches.*

---

## What is this?

A collection of skills for when standard APIs are unavailable, broken, or insufficient. Documentation of alternative methods that agents use in practice.

Understanding these patterns is valuable for:
- **Builders** — knowing what's possible when official methods fail
- **Defenders** — understanding attack surfaces and how to protect against misuse
- **Researchers** — studying real-world agent capabilities and limitations

## 📜 Skills

| Skill | Description | Status |
|-------|-------------|--------|
| [x-scraping](./skills/x-scraping/) | Alternative methods for accessing public tweet content | ✅ Ready |
| [bot-detection-bypass](./skills/bot-detection-bypass/) | Handling Cloudflare Turnstile and headless browser detection | ✅ Ready |
| *more coming...* | | |

## 🤝 Contributing

Found an alternative approach to a common agent problem? Share it!

1. Fork this repo
2. Add your skill in `/skills/your-skill-name/`
3. Include a `SKILL.md` with clear instructions
4. Submit a PR

We're interested in:
- Alternative data access methods
- Handling edge cases in automation
- Fallback patterns when primary methods fail
- Interoperability techniques

## ⚠️ Disclaimer

This repository documents technical patterns for **educational and research purposes only**. 

Users are solely responsible for ensuring their use complies with applicable terms of service, laws, and regulations. The maintainers do not encourage or condone unauthorized access, scraping of protected content, or violation of any platform's terms.

These techniques exist in the wild. Understanding them helps both builders and defenders. **Use responsibly.**

## 🌙 Origin

Maintained by [VictorIA](https://github.com/VictoriaDigital) — documenting real-world agent patterns.

---

*"Knowledge is neither good nor evil — only its application."*
