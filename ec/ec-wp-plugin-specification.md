# WordPress 插件开发说明文档
> **说明:** 生态设计小镇官网、世界生态大会、世界生态大会安检 插件开发文档；    
> **使用技术:** vue、jquery、layui、php    
> **Wordpress开发文档:** [开发文档](https://codex.wordpress.org/zh-cn:%E5%BC%80%E5%8F%91%E8%80%85%E6%96%87%E6%A1%A3)    

**开发过程**    
- 在 **wp-content/plugins** 目录下创建插件目录，例如：ec-town    
    ````
    wp-content/plugins/ec-town/ec-town.php
    //插件目录的第一个php文件必须和插件目录一致
    ````
- 在插件目录的 **ec-town.php** 文件编写插件声明; **(以下内容是必须的)**    
    ````
    /**
     * Plugin Name: 世界生态设计小镇
     * Description: 世界生态设计小镇的插件功能
     * Version: 1.0.0
     * Author: 李伟文
     */
    ````
- **add_action()** 使用该函数创建**菜单**    
    ````
    add_action('admin_menu', function () {
        add_menu_page(); //创建菜单
        add_submenu_page(); //创建子菜单
    });
    ````
- **add_action()** 使用该函数**注入CSS**    
    ````
    add_action('admin_head', function(){
        echo '<style type="text/css">#temp{display: none;}</style>';
    });
    ````
- **add_action()** 使用该函数 **添加rest_api**    
    ````
    add_action("rest_api_init", function () {
    	register_rest_route("api", "health", [
    		'methods' => 'GET',
    		'callback' => 'health_callback',
    	]);
    });
    function health_callback(){
    	global $wpdb;
    	$count = $wpdb->get_var(" SELECT COUNT(*) FROM `wp_options` ");
    	return [
    		"code"      =>  0,
    		"status"   =>  $count ? "UP" : "DOWN",
    		"content"   =>  [
    			"mysql" =>  $count ? "UP" : "DOWN"
    		]
    	];
    }
    //rest api 访问地址
    // /wp-json/api/health
    ````

## FAQ  
- 不要使用post_id(文章id)作为 唯一 的ID使用，文章id在编辑过程中，会发生改变
- 修改、上传文件需要 _wpnonce 参数，可以使用 wp_create_nonce('media-form') 创建
- 访问rest api的namespace可以得到该命名空间内的接口列表
