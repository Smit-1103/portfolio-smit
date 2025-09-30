# Portfolio Website – Smit Patel

Modern, fast portfolio built with **Next.js + React + TypeScript + TailwindCSS**.  
Light theme by default. Smooth animations. Resume preview and one‑click download.

## Highlights
- Responsive, accessible, SEO‑ready.
- Section reveal animations and page transitions tuned for zero jank.
- **Dark mode removed**. Light theme only.
- Resume page renders `/public/resume.pdf` inline + **Download Resume** button.
- All external links wired: GitHub, LinkedIn, Email, project repos, blog posts.
- Lighthouse targets: Performance ≥95, Accessibility ≥95, SEO ≥95.

---

## Tech
- Next.js (App Router or Pages, both supported)
- React 18
- TypeScript
- TailwindCSS
- MDX (blog)
- Plausible (optional analytics)

---

## Quick start
```bash
pnpm i   # or npm i / yarn
pnpm dev # http://localhost:3000
```
Build:
```bash
pnpm build && pnpm start
```

---

## Content model
Static data lives under `/data`:
- `data/profile.ts` – name, headline, social links.
- `data/tech.ts` – categorized tech stack.
- `data/experience.ts` – roles and bullets.
- `data/projects.ts` – six curated projects.
- `data/kpis.ts` – 40% faster, $15k/yr saved, 20+ countries.
- `content/blog/*.mdx` – blog posts.
- `/public/resume.pdf` – resume file served for preview and download.

### Seed content (current)
**Headline:** Data Analyst / Data Engineer — SQL, Python, Power BI, AWS.  
**KPIs:** 40% faster reporting • $15k/yr license savings • 20+ countries supported.  
**Experience:**
- **Data Analyst Intern — University of Windsor (May 2025–Present)**  
  Analytics across 20+ countries. Automated reimbursement approvals (‑40% cycle time). License model redesign (save $15k/yr). KPI dashboards and ROI by campaign.
- **Full Stack Developer / QA Automation — BarodaWeb (Jan 2023–Apr 2024)**  
  SQL validation pipelines (‑40% defect detection time). BI dashboards. Client analytics improvements.

**Education:**  
- Master of Applied Computing, University of Windsor (May 2024–Dec 2025, 88.4%).  
- B.E. Computer Science, GTU (Jul 2019–May 2023, 87.5%).

**Projects (Top 6):**
1) Recruitment Data Workflow Automation — Power BI, SQL, Excel, Jira.  
2) Data Pipeline Monitoring App — React Native, AWS, Kafka.  
3) Reporting Automation Pipeline — Python, AWS Lambda, SQL.  
4) License Optimization Analyzer — Power BI, Python, REST APIs.  
5) Sales BI Dashboard — Power BI, SQL.  
6) Data Quality Monitor — Python, Pandas.

---

## Required links
Update in `data/profile.ts`:
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

---

## Resume preview + download
- Put your PDF at `/public/resume.pdf`.
- Use this component (already referenced on `/resume`).

```tsx
// components/ResumeViewer.tsx
"use client";
import { useEffect, useRef } from "react";

export default function ResumeViewer() {
  const ref = useRef<HTMLIFrameElement>(null);
  useEffect(() => {
    // noop, keeps client component behavior explicit
  }, []);
  return (
    <div className="w-full">
      <div className="mb-4 flex gap-2">
        <a
          href="/resume.pdf"
          download
          className="rounded-md border px-3 py-2 text-sm hover:shadow"
          aria-label="Download Resume PDF"
        >
          Download PDF
        </a>
        <button
          onClick={() => window.print()}
          className="rounded-md border px-3 py-2 text-sm hover:shadow"
          aria-label="Print Resume"
        >
          Print
        </button>
      </div>
      <iframe
        ref={ref}
        src="/resume.pdf#view=FitH"
        className="h-[80vh] w-full rounded-lg border"
        title="Resume preview"
      />
    </div>
  );
}
```

Add a print stylesheet for cleaner printouts:
```css
/* styles/print.css */
@media print {
  nav, footer, .no-print { display: none !important; }
  iframe { height: 100vh !important; }
}
```

Import in `_app.tsx` or layout:
```ts
import "@/styles/print.css";
```

---

## Remove dark mode (force light theme)
- Delete any theme toggles and `dark:` variants.  
- Tailwind config:
```js
// tailwind.config.js
module.exports = {
  darkMode: false, // or remove 'class' setting
  content: ["./pages/**/*.{ts,tsx}", "./components/**/*.{ts,tsx}", "./app/**/*.{ts,tsx}"],
  theme: { extend: {} },
  plugins: []
};
```

If classes like `dark:bg-…` exist, keep the light equivalents only. Cursor rules below automate this.

---

## Animations
- Page transition: 200–250ms fade + slight translateY.
- Section reveal: 60ms stagger. Disable heavy parallax on mobile.
- Respect `prefers-reduced-motion`.

Example utility:
```tsx
export const fadeIn = "opacity-0 translate-y-3 motion-safe:animate-[fadein_220ms_ease-out_forwards]";
/* in globals.css
@keyframes fadein { to { opacity: 1; transform: translateY(0) } }
@media (prefers-reduced-motion: reduce) {
  * { animation: none !important; transition: none !important; }
}
*/
```

---

## SEO
- Update `/app/layout.tsx` metadata.
- Add `/public/og.jpg`, `sitemap.xml`, `robots.txt`.
- JSON‑LD `Person` + `Project` in the homepage.

---

## Accessibility
- Keyboard focus visible at all times.
- Skip‑to‑content link at top of layout.
- ARIA labels on social icons and buttons.
- Min contrast 3:1 for UI and 4.5:1 for text.

---

## Deployment
- Vercel: `npm run build` → deploy.  
- Set `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` if using analytics.

---

## Maintenance checklist
- [ ] Resume updated in `/public/resume.pdf`.
- [ ] Links validated (GitHub, LinkedIn, Email, repo links).
- [ ] Six projects max and current.
- [ ] Lighthouse ≥95. Axe checks pass.
- [ ] Blog posts tagged and dated.
