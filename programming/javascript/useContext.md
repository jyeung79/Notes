# useContext()

The hooks version of `static theme = createContext(ThemeContext)` where you can set hooks to your nearest `<ThemeContext.provider>` without passing it in your `return` or `render` statements.

```Javascript
import { ThemeContext } from '../pathTo/ThemeContextProvider'
import react, { useContext } from 'react';

const theme = useContext(ThemeContext);
```
`useContext` requires a default `context` object so you have to import or set it in your props. This exposes your `context objext` to your entire function component.

* useContext should be declared in your top-level component
* ie. I had issues with declaring it inside child components for `UpcomingPayments` and `PastPayments`
* Declaring it inside the `<PaymentItem>` gave issues so I gave up. I declared it inside `SectionList` function instead
* Had to place all of my functions inside `renderSectionList`

#usecontext #hooks-issues #boro #payments-history