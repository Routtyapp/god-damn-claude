# Metallic Effect

## 1. Metallic Button

```html
<button class="metallic-button">Download</button>
```

```css
.metallic-button {
  font-size: 14px;
  padding: 6px 16px;
  font-weight: 400;
  border: none;
  outline: none;
  color: #000;
  background: linear-gradient(
    45deg,
    #999 5%,
    #fff 10%,
    #ccc 30%,
    #ddd 50%,
    #ccc 70%,
    #fff 80%,
    #999 95%
  );
  text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.5);
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

.metallic-button:hover {
  transform: translateY(-2px);
}
```
