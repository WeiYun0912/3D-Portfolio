# 3D-Portfolio

[影片](https://www.youtube.com/watch?v=0fYi8SGA20k)

# 重點

## HOC

HOC 會根據使用的方式不同而有不同的參數，較常看到以下的方法：

```js
const SectionWrapper = ({ children }) => (
  <motion.section
    variants={staggerContainer()}
    initial="hidden"
    whileInView="show"
    viewport={{ once: true, amount: 0.25 }}
    className={`${styles.padding} max-w-7xl mx-auto relative z-0`}
  >
    <span className="hash-span">&nbsp;</span>
    {children}
  </motion.section>
);

export default SectionWrapper;
```

然後要使用的時候該 HOC 的時候，就在檔案引入，並在最上方包住整個 Component，像是這樣：

```js
const About = () => {
  return <SectionWrapper>....</SectionWrapper>;
};

export default About;
```

但在這支影片學到另外一種做法，也是之前在其他套件有看到的做法，直接使用 function return function 的方式來實作 HOC：

```js
// 接收其他參數 第一個是 Component 後面可以自行添加
const SectionWrapper = (Component, idName) =>
  function HOC() {
    return (
      <motion.section
        variants={staggerContainer()}
        initial="hidden"
        whileInView="show"
        viewport={{ once: true, amount: 0.25 }}
        className={`${styles.padding} max-w-7xl mx-auto relative z-0`}
      >
        <span className="hash-span" id={idName}>
          &nbsp;
        </span>
        <Component />
      </motion.section>
    );
  };
```

這次使用的方式不同，直接將 Wrapper 做為 function 來使用：

```js
const About = () => {
  return <>...</>;
};

export default SectionWrapper(About, "about");
```

## Media Query

使用 useEffect 搭配 BOM 的 function，來判斷是否為手機尺寸。

```js
const [isMobile, setIsMobile] = useState(false);

useEffect(() => {
  // Add a listener for changes to the screen size
  const mediaQuery = window.matchMedia("(max-width: 570px)");

  // Set the initial value of the `isMobile` state variable
  setIsMobile(mediaQuery.matches);

  // Define a callback function to handle changes to the media query
  const handleMediaQueryChange = (event) => {
    setIsMobile(event.matches);
  };

  // Add the callback function as a listener for changes to the media query
  mediaQuery.addEventListener("change", handleMediaQueryChange);

  // Remove the listener when the component is unmounted
  return () => {
    mediaQuery.removeEventListener("change", handleMediaQueryChange);
  };
}, []);
```

之後就可以判斷目前是否為手機尺寸，來設定元素的 CSS。

```js
<primitive
  object={computer.scene}
  scale={isMobile ? 0.05 : 0.1}
  position={isMobile ? [0, -0.8, -0.1] : [0, -2.5, 0.2]}
  rotation={isMobile ? [-0.01, 1.3, 0] : [-0.01, 1.1, 0]}
/>
```
