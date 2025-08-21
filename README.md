# Curvomorphism: A "New" Design Language

## What Even Is Curvomorphism?

Curvomorphism is basically treating corner rounding as **directional** instead of uniform. Normally, corners get the same rounding all around. With curvomorphism, corners get rounded depending on **where they are on the screen** and which way they “face.”

Inside corners (toward the screen’s center)? Rounded.
Outside corners (toward edges)? Sharp.

It creates this subtle “pull” toward the center of the UI. Everything feels soft and cohesive without losing structure.

## Traditional vs Curvomorphism

### Old School

```
All corners the same:
╭─────────╮← Rounded all around
│         │
│         │
╰─────────╯
```

### Curvomorphic

```
Corners depend on position:

Top-left element:          Top-right element:
┌─────────┐               ┌─────────┐← Outer corners sharp
│         │               │         |  
│         │               │         |
└─────────╯               ╰─────────┘
           ↑______________↑__ Rounded inner
            
┌─────────╮               ╭─────────┐
│         │               │         │
│         │               │         │
└─────────┘               └─────────┘
Bottom-left element:       Bottom-right element:
```
an SVG example:  
  
![curvomorphism](https://github.com/user-attachments/assets/23b9bc1d-12af-4868-a50e-ba244c07acda)
  
  
## Why Bother?

**Visual Hierarchy**

* Guides the eye naturally to the center (i'd assume)
* Creates distinct sections from each curvomorphic menu

**Directional Flow**

* Smooth, intuitive transitions (easing in and out)
* Gives subtle navigation cues

**Aesthetic-ness**

* Recognizable and modern
* Feels clean and intentional

## How To Do It

1. **Find Your Center**
   Pick the screen or content focal point. That’s your “inside” reference.

2. **Check Each Element**
   Which corners face the center? Those get the rounding.

3. **Apply Rounding**

* Inner corners → round (4–12px works)
* Outer corners → sharp (0px)

4. **Account for Rotation**
   Rotated elements round according to their orientation, not the grid.

## Where It Works

* **Menus**: dropdowns and sidebars with inner corners rounded
* **Modals**: corners toward center only, feels integrated
* **Cards**: grid items naturally lead the eye inward
* **Toolbars**: floating or side panels blend in subtly

## Quick CSS Example

```css
.curvomorphic-element {
  background: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 0; /* default sharp */
}

.position-top-left {
  border-bottom-right-radius: 12px;
}

.position-top-right {
  border-bottom-left-radius: 12px;
}

.position-center {
  border-radius: 12px; /* all rounded if center */
}
```

## Dynamic JS Version

```javascript
function applyCurvomorphism(element, centerX, centerY) {
  const rect = element.getBoundingClientRect();
  const elCenterX = rect.left + rect.width / 2;
  const elCenterY = rect.top + rect.height / 2;
  
  const r = '12px', s = '0px';
  
  const leftIn = elCenterX > centerX;
  const rightIn = elCenterX < centerX;
  const topIn = elCenterY > centerY;
  const bottomIn = elCenterY < centerY;
  
  element.style.borderTopLeftRadius = (topIn && leftIn) ? r : s;
  element.style.borderTopRightRadius = (topIn && rightIn) ? r : s;
  element.style.borderBottomLeftRadius = (bottomIn && leftIn) ? r : s;
  element.style.borderBottomRightRadius = (bottomIn && rightIn) ? r : s;
}
```

## Design System Tips

* **Variants:** `.element--top-left`, `.element--center`, etc.
* **Responsive:** Recalculate for different screens, fallback to normal rounding for tiny screens.
* **Consistency:** Keep radius values predictable across your UI.
* **Subtlety First:** Start small (4px) before going extreme.
* **Accessibility:** Make sure corners don’t mess with touch targets or screen readers.

## Real-Life Examples

* **HTMLPlayer v2:** Currently in beta, but fully uses this style.
* **iOS Shortcuts and a lot of iOS windows:** top corners rounded inward, bottom square

---

*Curvomorphism term and concept created by NellowTCS.*
