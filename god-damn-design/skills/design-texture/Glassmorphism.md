# Glassmorphism

## 1. 기본 CSS

```css
.glass-card {
  /* 배경 흐림 효과 (핵심) */
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  /* 투명도 있는 배경과 테두리 */
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);

  /* 입체감을 위한 그림자 */
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
  border-radius: 15px;
  padding: 40px;
  color: white;
}
```

## 2. Animated 3D Glass Button

```css
@property --angle-1 { syntax: "<angle>"; inherits: false; initial-value: -75deg; }
@property --angle-2 { syntax: "<angle>"; inherits: false; initial-value: -45deg; }

/* 3D 질감을 위한 복합 마스킹 기법 */
.button-after {
  background: conic-gradient(from var(--angle-1) at 50% 50%, rgba(0,0,0,0.5), rgba(0,0,0,0) 5% 40%, rgba(0,0,0,0.5) 50%, rgba(0,0,0,0) 60% 95%, rgba(0,0,0,0.5));
  mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  mask-composite: exclude;
}
```

## 3. 재사용 컴포넌트: GlassButton

```tsx
import React from 'react';
import './GlassButton.css';

interface GlassButtonProps {
  label: string;
  onClick?: () => void;
  width?: string;
  className?: string;
}

export const GlassButton: React.FC<GlassButtonProps> = ({
  label, onClick, width, className = ''
}) => {
  return (
    <div className={`button-wrap ${className}`} style={{ width }}>
      <div className="button-shadow"></div>
      <button onClick={onClick} className="glass-premium-button">
        <span>{label}</span>
      </button>
    </div>
  );
};
```

## 4. 재사용 컴포넌트: GlassCard

```tsx
import React, { ReactNode } from 'react';

interface GlassCardProps {
  children: ReactNode;
  width?: string;
}

export const GlassCard: React.FC<GlassCardProps> = ({ children, width = '100%' }) => {
  const cardStyle: React.CSSProperties = {
    position: 'relative',
    background: 'rgba(255, 255, 255, 0.15)',
    backdropFilter: 'blur(10px)',
    border: '1px solid rgba(255, 255, 255, 0.3)',
    boxShadow: '0 8px 32px rgba(0, 0, 0, 0.2)',
    borderRadius: '20px',
    width: width,
    padding: '2rem',
    overflow: 'hidden'
  };

  return <div style={cardStyle}>{children}</div>;
};
```
