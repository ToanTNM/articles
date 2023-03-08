# ReactJS with TypeScript

Reference:

- <https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/function_components>

- <https://github.com/gndelia/codemod-replace-react-fc-typescript>

1. Type & Interface

    ```ts
    type AppProps = {
      message: string;
      count: number;
      disabled: boolean;
      /**array of a type! */
      names: string[];
    /** string literals to specify exact string values, with a union type to join them together*/
      status: "waiting" | "success";
      /**an object with known properties (but could have more at runtime) */
      obj: {
        id: string;
        title: string;
      };
    /** array of objects! (common)*/
      objArr: {
        id: string;
        title: string;
      }[];
      /**any non-primitive value - can't access any properties (NOT COMMON but useful as placeholder) */
      obj2: object;
    /** an interface with no required properties - (NOT COMMON, except for things like `React.Component<{}, State>`)*/
      obj3: {};
      /**a dict object with any number of properties of the same type */
      dict1: {
        [key: string]: MyTypeHere;
      };
      dict2: Record<string, MyTypeHere>; // equivalent to dict1
    /** function that doesn't take or return anything (VERY COMMON)*/
      onClick: () => void;
      /**function with named prop (VERY COMMON) */
      onChange: (id: number) => void;
    /** function type syntax that takes an event (VERY COMMON)*/
      onChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
      /**alternative function type syntax that takes an event (VERY COMMON) */
      onClick(event: React.MouseEvent<HTMLButtonElement>): void;
    /** any function as long as you don't invoke it (not recommended)*/
      onSomething: Function;
      /**an optional prop (VERY COMMON!) */
      optional?: OptionalType;
    /** when passing down the state setter function returned by `useState` to a child component. `number` is an example, swap out with whatever the type of your state*/
      setState: React.Dispatch<React.SetStateAction<number>>;
    };
    ```

    ```js
      export declare interface AppProps {
        children?: React.ReactNode; // best, accepts everything React can render
        childrenElement: JSX.Element; // A single React element
        style?: React.CSSProperties; // to pass through style props
        onChange?: React.FormEventHandler<HTMLInputElement>; // form events! the generic parameter is the type of event.target
        //  more info: <https://react-typescript-cheatsheet.netlify.app/docs/advanced/patterns_by_usecase/#wrappingmirroring>
        props: Props & React.ComponentPropsWithoutRef<"button">; // to impersonate all the props of a button element and explicitly not forwarding its ref
        props2: Props & React.ComponentPropsWithRef<MyButtonWithForwardRef>; // to impersonate all the props of MyButtonForwardedRef and explicitly forwarding its ref
    }
    ```

1. Function Component

    ```ts
    // Declaring type of props - see "Typing Component Props" for more examples
    type AppProps = {
      message: string;
    }; /*use `interface` if exporting so that consumers can extend*/

    // Easiest way to declare a Function Component; return type is inferred.
    const App = ({ message }: AppProps) => <div>{message}</div>;

    // you can choose annotate the return type so an error is raised if you accidentally return some other type
    const App = ({ message }: AppProps): JSX.Element => <div>{message}</div>;

    // you can also inline the type declaration; eliminates naming the prop types, but looks repetitive
    const App = ({ message }: { message: string }) => <div>{message}</div>;
    ```

    Conditional rendering is not support:

    ```ts
    const MyConditionalComponent = ({ shouldRender = false }) =>
      shouldRender ? <div /> : false; // don't do this in JS either
    const el = <MyConditionalComponent />; // throws an error
    ```

    Array.fill

    ```ts
    const MyArrayComponent = () => Array(5).fill(<div />);
    const el2 = <MyArrayComponent />; // throws an error

    //-------> must be type conversion
    const MyArrayComponent = () => Array(5).fill(<div />) as any as JSX.Element;
    ```

1. Hooks

- useState

    ```ts
    const [state, setState] = useState(false);
    // `state` is inferred to be a boolean
    // `setState` only takes booleans

    const [user, setUser] = useState<User | null>(null);

    // later...
    setUser(newUser);
    ```

- useCallback

    ```ts
    const memoizedCallback = useCallback(
      (param1: string, param2: number) => {
        console.log(param1, param2)
        return { ok: true }
      },
      [...],
    );
    /**
     * VSCode will show the following type:
    * const memoizedCallback:
    *  (param1: string, param2: number) => { ok: boolean }
    */
    ```

- useReducer

    ```ts
    import { useReducer } from "react";

    const initialState = { count: 0 };

    type ACTIONTYPE =
      | { type: "increment"; payload: number }
      | { type: "decrement"; payload: string };

    function reducer(state: typeof initialState, action: ACTIONTYPE) {
      switch (action.type) {
        case "increment":
          return { count: state.count + action.payload };
        case "decrement":
          return { count: state.count - Number(action.payload) };
        default:
          throw new Error();
      }
    }

    function Counter() {
      const [state, dispatch] = useReducer(reducer, initialState);
      return (
        <>
          Count: {state.count}
          <button onClick={() => dispatch({ type: "decrement", payload: "5" })}>
            -
          </button>
          <button onClick={() => dispatch({ type: "increment", payload: 5 })}>
            +
          </button>
        </>
      );
    }
    ```
