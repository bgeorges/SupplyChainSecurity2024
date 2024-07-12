# Software Supply Chain Security

A Presentation about software Supply Chain Security. While most of the content is focused on Java (OpenJDK, Quarkus, etc...), SSCS is language agnostc. We discuss the framweorks and the tools that are most commony used and provide some examples to illustrates their prupose.

I intend to add another part to cover the operaitonal side, i.e container scanning, signing, adn more. Stay tuned.

The presentation uses the MARP presentation framework. Follow the instructions below to generate a PDF (or other format).

Usage:

Install MARP cli

```
npm install -g @marp-team/marp-cli
```

Go to the location of your presentation

```
cd /path/to/marp-presentation
```

Serve your presentation

```
marp --serve presentation.md
```

Export as PDF 
```
marp presentation.md --pdf
```
