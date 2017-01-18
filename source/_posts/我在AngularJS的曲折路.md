---
title: 我在AngularJS的曲折路
date: 2017-01-17 15:18:55
tags:
---
新手单枪匹马闯AngularJS之路，走过的每个弯路，遇到的每个难题，都是汗和泪，再次记录下来，共勉。

1.实现AngularJS的国际化问题的时候，按照网上的步骤进行配置Angular-translate，进行了translate配置，切换控制器，设置支持JSON文件的多语过滤器。

多语文件目录 webapp/i18n/cn.json  webapp/i18n/cn.json


lang.translate.js 配置translateConfig
```python
(function() {
    'use strict';

    angular
        .module('jhipsterApp', ['pascalprecht.translate'])
        .config(translateConfig);

    translateConfig.$inject = ['$translateProvider'];

    function translateConfig($translateProvider) {
        var lang = window.localStorage.lang || 'cn';
        $translateProvider.preferredLanguage(lang);
        $translateProvider.useStaticFilesLoader({
            prefix: '/i18n/',
            suffix: '.json'
        });
    }
})();
```

lang.controller.js配置控制器
```python
(function() {
    'use strict';

    angular
        .module('jhipsterApp')
        .controller('LanguageSwitchingCtrl', LanguageSwitchingCtrl);

    LanguageSwitchingCtrl.$inject = ['$scope', '$translate'];

    function LanguageSwitchingCtrl($scope, $translate) {
        var vm = this;
        $scope.switching = function(lang) {
            $translate.use(lang);
            window.localStorage.lang = lang;
            window.location.reload();
        };
        // $scope.cur_lang = $translate.use();
        // vm.cur_lang = $translate.use();
        vm.cur_lang = "cn";
        $translate.use("cn");
        $scope.cur_lang = "cn";
        window.localStorage.lang = "cn";
    }
})();
```

T.js配置过滤器
```python
(function() {
	'use strict';

	angular
		.module('jhipsterApp')
		.filter('T',T);
		
	T.$inject = ['$translate'];

	function T($translate) {
		return function(key) {
			if (key) {
				return $translate.instant(key);
			}
		};
	}
})();
```

app.module.js这是原来的APP初始化方法
```python
(function() {
    'use strict';
    angular
        .module('jhipsterApp', [
            'ngStorage',
            'ngResource',
            'ngCookies',
            'ngAria',
            'ngCacheBuster',
            'ngFileUpload',
            'ui.bootstrap',
            'ui.bootstrap.datetimepicker',
            'ui.router',
            'infinite-scroll',
            // jhipster-needle-angularjs-add-module JHipster will add new module here
            'angular-loading-bar',
        ])
        .run(run);

    run.$inject = ['stateHandler'];

    function run(stateHandler) {
        stateHandler.initialize();
    }
})();
```

index.html中引入顺序
原引入
<script src="app/app.module.js"></script>

新引入
<script src="app/lang/lang.translate.js"></script>
<script src="app/lang/lang.controller.js"></script>
<script src="app/services/lang/T.js"></script>
发现新引入后置于app/app.module.js前后效果都不同，置于后面，app.module.js不能顺利执行，置于最前面，引入的模块不能生效，折磨了很久，加了N个技术群，查了N多资料，重新看了模块和注入依赖的内容，还是找不到根源。终于在长达6小时的折磨后，我猛然发现了问题。
lang.translate.js 中
angular.module('jhipsterApp', ['pascalprecht.translate'])
        .config(translateConfig);
这种写法是进行模块的配置，与app.module中 angular
        .module('jhipsterApp', [
            'ngStorage',
            'ngResource',
            'ngCookies',
            'ngAria',
            'ngCacheBuster',
            'ngFileUpload',
            'ui.bootstrap',
            'ui.bootstrap.datetimepicker',
            'ui.router',
            'infinite-scroll',
            // jhipster-needle-angularjs-add-module JHipster will add new module here
            'angular-loading-bar',
        ])有冲突！都在进行模块配置生成模块实例，会发生错误，导致后续的链式操作不能正常执行。

修改如下：
app.module.js
 angular
        .module('jhipsterApp', [
            'ngStorage',
            'ngResource',
            'ngCookies',
            'ngAria',
            'ngCacheBuster',
            'ngFileUpload',
            'ui.bootstrap',
            'ui.bootstrap.datetimepicker',
            'ui.router',
            'infinite-scroll',
            // jhipster-needle-angularjs-add-module JHipster will add new module here
            'angular-loading-bar',
            'pascalprecht.translate'
        ])
        .run(run);
lang.translate.js
 angular
        .module('jhipsterApp')
        .config(translateConfig);

测试成功，很感动，6个多小时的反复查看，就是基本概念没有掌握，这个弯路走的很深刻。
