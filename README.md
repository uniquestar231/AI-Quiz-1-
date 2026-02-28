Here’s a simple README.txt you can use for Question 3, tailored to your project and requirements:

text
Question 3: Prompt Engineering and Verification

1. Prompt 1: Planning prompt (no code)

Prompt sent to ChatGPT:
"Act as a senior front‑end developer. Help me plan how to implement responsive breakpoints and hero behavior for a marketing website. The layout should be mobile‑first. On small screens (roughly ≤600px), navigation should stack vertically and the hero text and card should stack. On medium screens (601–900px), the navigation should be horizontal and the card grid should show 2 columns. On large screens (≥901px), the grid should show 4 columns and the hero should be two‑column (text left, card right). Explain only the high‑level strategy: how many breakpoints to use, how to structure the hero so it stacks cleanly, and how to use flexbox or grid at each breakpoint. Do not write any code; respond only in plain English."

Two things I accepted:
- I accepted using three breakpoints: ≤600px (phone), 601–900px (tablet), and ≥901px (desktop), because they match common device sizes and keep the stylesheet simple.
- I accepted changing flex-direction from row to column on small screens for both the header and hero, so the layout stacks cleanly without extra wrappers.

One thing I rejected (and why):
- I rejected giving the hero card a fixed width (like width: 240px) on all screens because it wastes space on mobile. Instead, I used max-width: 240px and let it grow to 100% on small screens so the layout feels more flexible.

2. Prompt 2: Debug prompt (problem + CSS)

Problem statement:
"The hero section does not stack on narrow viewports; the text and hero-card stay side‑by‑side even below 600px, causing horizontal overflow and a horizontal scroll bar. The navigation also does not align correctly on mobile; it sticks to the left instead of being centered."

CSS snippet I asked about:
.hero {
  display: flex;
  padding: 20px 0;
  gap: 24px;
  align-items: center;
  justify-content: space-between;
}

.hero-card {
  width: 240px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 8px;
  text-align: center;
  background-color: #f5f5ff;
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav {
  display: flex;
}

Most important part of the AI response:
"On small screens, override the flex direction so text and card stack vertically. Add a media query that sets flex-direction: column on the hero and lets the card grow to full width. Also, set flex-direction: column and align-items: center on .nav so the links stack and center on mobile."

What I changed afterward:
- Added this for mobile:
  @media (max-width: 600px) {
    .header-inner {
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }

    .nav {
      flex-direction: column;
      align-items: center;
    }

    .hero {
      flex-direction: column;
      align-items: stretch;
    }

    .hero-card {
      width: 100%;
      max-width: 260px;
      align-self: center;
    }
  }
- Changed .hero-card from width: 240px to max-width: 240px so it can shrink or grow appropriately on different screens.

3. Verification list

Viewport sizes tested:
- 375px (phone / small screen)
- 768px (tablet / medium screen)
- 1200px (desktop)

What I checked visually at each size:
- 375px:
  - Navigation is stacked and centered beneath the logo, no horizontal overflow.
  - Hero text and hero-card are stacked vertically, with the card centered.
  - Grid is 1‑column, with cards stacked and no overflow.

- 768px:
  - Navigation is horizontal again, with logo on the left and nav on the right.
  - Hero text and card are side‑by‑side, with the card aligned to the right.
  - Grid is 2‑column; cards are evenly spaced and no horizontal overflow.

- 1200px:
  - Full desktop layout: header with logo left and nav right, hero in two columns, hero-card aligned to the right.
  - Grid is 4‑column; cards are evenly spaced and the container is centered at max-width: 1000px with padding on the sides.
