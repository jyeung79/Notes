# useMemo

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

Basically useMemo will only recompute the memoized value if the dependencies changes

```javascript
const memoizedCallback = useCallback(
  () => {
      doSomething(a, b);
  },
  [a, b],
);
```

useCallback will return a memoized version of the callback that only changes if one of the dependencies has changed. 

Both are related to reference equality.

#javascript #usememo #usecallback #hooks