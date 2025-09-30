# Cursor Instructions – Apply these edits automatically

> Paste this whole file into Cursor as the instruction context.  
> It performs refactors to enforce light theme, wire links, enable resume preview, and tune animations.

## 0) Project assumptions
- Next.js + React + TS + Tailwind.
- Files may live under `/app` or `/pages`. Detect automatically where routes exist.
- If a referenced file does not exist, create it.

---

## 1) Force light theme, remove dark mode
- Remove any theme provider and color-scheme toggles.
- In `tailwind.config.js` set `darkMode` to `false` or remove it entirely.
- Delete all `dark:` class variants. Keep the light fallback class only.
- Replace body background with a light neutral:
  - Ensure `body` has `bg-white text-neutral-900`.
- If a `ThemeToggle` component exists, remove it and its import/usage.

Search-replace map (safe):
- Find: `/\bdark:([^\s"]+)/g` → Replace: `` (remove)
- Find: `className="([^"]*?)\s?dark:[^"]*"` → Replace with the captured non-dark classes.

---

## 2) Wire global profile links
Create or update `data/profile.ts`:
```ts
export const profile = {
  name: "Smit Patel",
  headline: "Data Analyst / Data Engineer — SQL, Python, Power BI, AWS",
  email: "smitpatel7032@gmail.com",
  github: "https://github.com/Smit-1103",
  linkedin: "https://www.linkedin.com/in/smit-patel-34848a210/",
  location: "Kitchener, ON, Canada"
};
```
Update Header/Footer components to read from `profile` and ensure anchor tags have `rel="noopener noreferrer"` and `target="_blank"` where external.

---

## 3) Resume page: preview + download
- Ensure `/public/resume.pdf` exists. If not, create a placeholder and note in README.
- Create `components/ResumeViewer.tsx` (code provided in README).
- On route `/resume`, render `<ResumeViewer />`.
- Add a visible **Download PDF** button linking to `/resume.pdf` with `download` attribute.
- Add a **Print** button that calls `window.print()`.
- Add `styles/print.css` and import once globally.

---

## 4) Projects data and cards
Create or update `data/projects.ts` with six items only:
```ts
export const projects = [
  { title: "Recruitment Data Workflow Automation", outcome: "Unified attendance + cost ETL; 50% less manual effort.", tech: ["Power BI","SQL","Excel","Jira"], links: { repo: "", demo: "" }, tags: ["analytics","etl","dashboards"] },
  { title: "Data Pipeline Monitoring App", outcome: "Mobile monitoring; 35% faster incident response.", tech: ["React Native","AWS","Kafka"], links: { repo: "", demo: "" }, tags: ["monitoring","ops"] },
  { title: "Reporting Automation Pipeline", outcome: "Cut finance reporting cycles by 40%.", tech: ["Python","AWS Lambda","SQL"], links: { repo: "", demo: "" }, tags: ["automation","finance"] },
  { title: "License Optimization Analyzer", outcome: "Saved $15k/yr identifying unused licenses.", tech: ["Power BI","Python","REST"], links: { repo: "", demo: "" }, tags: ["cost","governance"] },
  { title: "Sales BI Dashboard", outcome: "Cross-filter sales + finance for better visibility.", tech: ["Power BI","SQL"], links: { repo: "", demo: "" }, tags: ["bi"] },
  { title: "Data Quality Monitor", outcome: "Anomaly checks; 60% fewer BI dataset failures.", tech: ["Python","Pandas"], links: { repo: "", demo: "" }, tags: ["quality"] }
];
```
- Update the Projects grid to read from this file.
- For any missing repo/demo links, keep buttons hidden with `aria-disabled="true"` to avoid dead links.

---

## 5) Experience & Education
Create or update `data/experience.ts` and `data/education.ts`:
```ts
export const experience = [
  {
    company: "University of Windsor",
    role: "Data Analyst Intern",
    period: "May 2025 – Present",
    bullets: [
      "Delivered analytics for campaigns across 20+ countries.",
      "Automated reimbursement approvals; reduced cycle time by 40%.",
      "Redesigned subscription usage; saved $15k/year.",
      "Built KPI dashboards and ROI by campaign."
    ]
  },
  {
    company: "BarodaWeb",
    role: "Full Stack Developer / QA Automation Engineer",
    period: "Jan 2023 – Apr 2024",
    bullets: [
      "Automated SQL validation pipelines; 40% faster defect detection.",
      "Built BI dashboards for defect density and KPIs.",
      "Improved client analytics and reporting transparency."
    ]
  }
];

export const education = [
  { school: "University of Windsor", degree: "Master of Applied Computing", period: "May 2024 – Dec 2025", notes: ["Current average: 88.4%"] },
  { school: "Gujarat Technological University", degree: "B.E. in Computer Science", period: "Jul 2019 – May 2023", notes: ["Final percentage: 87.5%"] }
];
```

---

## 6) Animations: keep subtle, performant
- Global keyframes: `fadein` with 220ms ease-out.
- Apply `motion-safe` utilities. Disable complex parallax on mobile.
- Add `prefers-reduced-motion` guard in CSS.

---

## 7) Accessibility
- Add `Skip to content` link before header and focus it on `Tab`.
- Ensure all interactive elements have visible focus.
- Add `aria-label` for social icon-only buttons and project cards.

---

## 8) SEO & metadata
- Set page `title` and `description` with name + headline.
- Add JSON-LD `Person` on home and `Article` on blog posts.
- Include `sitemap.xml` and `robots.txt` under `/public` if missing.

---

## 9) Cleanup dark styles and ensure light tokens
- Replace any `bg-neutral-900` with `bg-white` and `text-white` with `text-neutral-900`.
- Ensure cards use `border` and `shadow-sm` instead of heavy dark backgrounds.
- Replace gradients with subtle light neutrals if needed.

Codemod examples:
- Find: `bg-(slate|neutral|zinc)-9(00|50?)` → Replace with `bg-white`.
- Find: `text-(slate|neutral|zinc)-100` → Replace with `text-neutral-900`.

---

## 10) Validate links
- Ensure all `<a>` with `href` to external have `target="_blank"` and `rel="noopener noreferrer"`.
- For empty links, remove the element or add `aria-disabled="true"` and `tabIndex={-1}`.

---

## 11) Performance guard
- Preload the main font weight only.
- Use `next/image` for all images with width/height.
- Defer non-critical scripts. No layout shift on hero.

---

## 12) Deliverables checklist
- [ ] Light theme applied. Dark classes removed.
- [ ] Resume preview works. Download button works.
- [ ] Links wired: GitHub, LinkedIn, Email, repos.
- [ ] Projects limited to six with outcomes and tech.
- [ ] Experience and Education populated.
- [ ] Animations smooth; reduced-motion respected.
- [ ] Lighthouse and Axe checks ≥95.
