# wxxcxlogin
微信小程序登陆


前段时间刚好做了一个小程序的项目，用的框架是laravel

想想看，自己还没有写过composer的依赖包，刚好借这个机会试一下，功能比较简单，但是也刚好熟悉一下compoer的依赖包的书写规范，以及如何发布。

注册服务提供者到 Laravel中 具体位置：/config/app.php 中的 providers 数组:

quepq\wxxcx\WxxcxServiceProvider::class,

发布配置文件:

php artisan vendor:publish --tag=wxxcx


具体使用


use quepq\wxxcx\wxxcx;

class wxxcxController extends Controller
{
    protected $wxxcx;

    function __construct(Wxxcx $wxxcx)
    {
        $this->wxxcx = $wxxcx;
    }

    public function getWxUserInfo()
    {
        //code 在小程序端使用 wx.login 获取
        $code = request('code', '');
        //encryptedData 和 iv 在小程序端使用 wx.getUserInfo 获取
        $encryptedData = request('encryptedData', '');
        $iv = request('iv', '');

        //根据 code 获取用户 session_key 等信息, 返回用户openid 和 session_key
        $userInfo = $this->wxxcx->getLoginInfo($code);

        //获取解密后的用户信息
        return $this->wxxcx->getUserInfo($encryptedData, $iv);
    }
}
