# cameraTest
 pc上可以视频实现录屏的案例
# 0. 准备动作 

# 1. 初始化项目

#  2. 安装依赖

 webpack 的相关问题：
 常见的loader;
 常见的plugin;
 可以提高效率的插件(模板的热替换);
 loader(函数)与plugin(插件,插件可以扩展 Webpack 的功能)的区别；
 webpack的编译原理；
 初始化： 启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler
 编译： 从 Entry 出发，针对每个 Module 串行调用对应的 Loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理
 输出： 将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中
 sourcemap的原理与用法(将编译、打包、压缩后的代码映射回源代码的过程。;;通过nginx设置);
 文件监听的原理(轮询监听文件最后编辑时间，有变动过先缓存，等延迟时间到了再执行);
 webpack的热更新原理(WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。将差异的内容通过jsonp更新)；
 文件指纹(打包后输出的文件名的后缀，包括图片，js,css类型);
 编写loader的思路--；
 编写plugin的思路--;
 babel原理--；
 食物：黄瓜，苹果，苦瓜；
 一些js函数编写例子：
 function money(str) {
  const len = str.length;
  let start = len % 3 || 3;
  console.log('start',start)
  let arr = [str.slice(0, start)];
  console.log('arr',start)

  while(start < len) {
    arr.push( str.slice(start, start + 3) );
    start += 3;
  }
  console.log('arr1',arr)

  return arr.join(',')
}
    ' hi man good  luck '.replace(/\w+/g, function(word) { 
        console.log('word',word)
        return word.substr(0,1).toUpperCase() + word.substr(1);
    });
'aaabbbcdfgghhjjkkk'.replace(/([A-Za-z]{1})(\1)+/g, '$2');
promise.all的写原生函数
Promise._all = function (promises) {
  return new Promise(function(resolve, reject) {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('arguments must be an array'));
    }
    const len = promises.length;
    let cnt = 0;
    let res = [];
    for(let i = 0; i < len; i++) {
      Promise
        .resolve(promises[i])
        .then(function(value) {
          cnt++;
          res[i] = value;
          if (cnt === len) {
            return resolve(res);
          }
        }, function(reason) {
          return reject(reason);
        });
    }
  });
};
jsonp 的原理方法
function fn({ip}) {
  console.log(ip); // 
}
function jsonp(cb, domain) {
  const script = document.createElement('script');
  script.src = `https://api.asilu.com/ip/?callback=${cb}&ip=${domain}`;
  document.querySelector('head').appendChild(script);
}

// 获取百度IP
jsonp('fn', 'www.baidu.com');//220.181.38.149
限制并发数量
function sendRequest(urls, max, callback) {
  let last = performance.now();
  let len = urls.length;
  let limit = max; // 控制并发数
  let cnt = 0;     // 累积执行任务数
  let res = [];    // 有序存储执行结果
  const tasks = urls.map((url, index) => () => fetch(url)
    .then(data => {
      res[index] = data;
    })
    .catch(reason => {
      res[index] = reason;
    })
    .finally(() => {
      if( ++cnt === len ) return callback(res);
      ++limit;
      doTasks();
    }));

  doTasks();

  function doTasks() {
    while( limit && tasks.length ) {
      --limit;
      let task = tasks.shift();
      task();
      console.log(`执行间隔：${performance.now() - last}`);
    }
  }
}

// 模拟 fetch
function fetch(url) {
  return new Promise(function (resolve, reject) {
    let good, bad;
    const time = 3000;

    good = setTimeout(function () {
      clearTimeout(bad);
      let data = `resolve: ${url}`;
      resolve(data);
      console.log(data);
    }, Math.random() * time);

    bad = setTimeout(function () {
      clearTimeout(good);
      let reason = `reject: ${url}`;
      reject(reason);
      console.log(reason);
    }, Math.random() * time);
  });
}

// 测试
sendRequest([1,2,3,4,5,6,7,8,9,10], 5, (res) => console.log('all done：' + res));
关于bfc(块级格式化上下文)的理解与例子。
前端的性能异常监控：
(后台日志，try catch,埋点Js文件，IE外使用window.onerror)
#  3. 关于张爱玲的
语录：语录分析
没有几个女人是因为灵魂之美而被爱上的。--偏偏
只说银河是泪水，原来银河轻浅确实形容喜悦。--浅浅
我喜欢钱，因为我没有吃过钱的苦，不知道钱的坏处，只知道钱的好处。--张
如果你认识从前的我，你就会原谅现在的我。--张
向左走，向右走  --几米献给注定要相遇的人
忘记亲一下 --你永远不可记起的富士山凤尾草殿下
我要你知道这世界总有一个人是在等你的。 --测试的
什么都想问，又什么都不想问。 --x
你还没有来，我怎么敢老去。 --张爱玲的胡兰成先生适用的咸鱼
饮鸩止渴 --长恨歌别离 凄惨参芪



