---
order: 7
title:
  zh-CN: 输入时格式化展示
  en-US: Format Tooltip Input
---

## zh-CN

结合 [Tooltip](/components/tooltip) 组件，实现一个数值输入框，方便内容超长时的全量展现。

## en-US

You can use the Input in conjunction with [Tooltip](/components/tooltip) component to create a Numeric Input, which can provide a good experience for extra-long content display.

```jsx
import { Input, Tooltip } from 'antd';

function formatNumber(value) {
  return new Intl.NumberFormat().format(value);
}

const NumericInput = props => {
  const { value, onBlur, onChange } = props;

  const handleChange = e => {
    const inputValue = e.target.value;
    const reg = /^-?\d*(\.\d*)?$/;
    if ((!isNaN(inputValue) && reg.test(inputValue)) || inputValue === '' || inputValue === '-') {
      onChange(inputValue);
    }
  };

  // '.' at the end or only '-' in the input box.
  const handleBlur = () => {
    let valueTemp = value;
    if (value.charAt(value.length - 1) === '.' || value === '-') {
      valueTemp = value.slice(0, -1);
    }
    onChange(valueTemp.replace(/0*(\d+)/, '$1'));
    if (onBlur) {
      onBlur();
    }
  };

  const title = value ? (
    <span className="numeric-input-title">{value !== '-' ? formatNumber(value) : '-'}</span>
  ) : (
    'Input a number'
  );
  return (
    <Tooltip trigger={['focus']} title={title} placement="topLeft" overlayClassName="numeric-input">
      <Input
        {...props}
        onChange={handleChange}
        onBlur={handleBlur}
        placeholder="Input a number"
        maxLength={25}
      />
    </Tooltip>
  );
};

export default () => {
  const [value, setValue] = React.useState();

  const onChange = val => {
    setValue(val);
  };

  return <NumericInput style={{ width: 120 }} value={value} onChange={onChange} />;
};
```

```css
/* to prevent the arrow overflow the popup container,
or the height is not enough when content is empty */
.numeric-input .ant-tooltip-inner {
  min-width: 32px;
  min-height: 37px;
}

.numeric-input .numeric-input-title {
  font-size: 14px;
}
```
