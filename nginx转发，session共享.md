
# nginxת����session����


��nginxת����ͨ�������cookie��session��һЩ���⡣����ʵ�ֵ���һ����nginxת���󲢱���session������������������һ��session���Ӷ��÷��������һЩ����ͨ�š���ȥһЩ����Ȩ���жϡ�

![|center](../master/src/1487846181327.png)



## ʵ������
>A������������nginxת����

>B������������ת����Ĵ���ʵ��

>Redis���������洢���з�����������session�������Ǳ��������cookie���sessionId����thinkphp������
    

## ���̣�
1.��A�������ϵ�������Ŀ���������ִ������һЩ����������session���洢��redis�

>	���������session�����thinkphp��ܲ�������Ϊtp��ܻ������������ɵ�sessionIdΪkey����Ϣ�洢�ڱ��������cookie��redis�������

> ȥredis��ȡsession��Ҫ����sessionId��

> sessionId����session_start()ʱ���ͻ��Զ�����session_id()


2.��A�������Ϸ���nginx����ת����Ŀ¼���ᱻnginx�� proxy_pass ת����Ŀ���������B���������Ķ�Ӧ�ļ���
3.B�������ϵĴ���ʵ��һ�����ܣ�չʾ��ǰsession����Ϣ������õ�ǰ����������ת����������ת��������Ȼ��ǰ��������ת�����B�����������ǡ���ǰ��������������ת��ǰ��A����������cookie���sessionIdȥredis��ѯ������ת��ǰ���Ѿ��洢��session



## ��ʵ�ֲ��֣�

A��������nginx�������ļ� /usr/local/nginx/conf/nginx.conf ���server��������´��룬����nginxת��
```
   location /ops/ {
                proxy_pass http://B������IP/think/public/;
        }
```

��A��B��������thinkphp��ܵ� /application/config.php �ļ�������´��룬ʵ��session��redis��
```
    // +----------------------------------------------------------------------
    // | �Ự����
    // +----------------------------------------------------------------------

    'session'                => [
        'id'             => '',
        // SESSION_ID���ύ����,���flash�ϴ�����
        'var_session_id' => '',
        // SESSION ǰ׺
        'prefix'         => 'think',
        // ������ʽ ֧��redis memcache memcached
        'type'           => 'redis',
        // �Ƿ��Զ����� SESSION
        'auto_start'     => true,
        // redis����
        'host'       => '192.168.9.142', //redis��������IP
        // redis�˿�
        'port'       => 6379,
        // ����
        'password'   => '',
    ],
```

## ����

A��������sessionд�����
>http://192.168.9.142/think/public/?s=/index/index/set&key=user&value=20170227
index�ļ���set������ʵ��sessionд��
```
  public function set()
    {
        $key = $_GET['key'];
        $vlaue = $_GET['value'];
        Session::set($key,$vlaue);
        echo "session��set����,д��ɹ���<hr>��ȡ".$key."���õ�";
        return '';
    }

```

A��������nginx����ops����ת����B���������н���
>http://192.168.9.142/ops/?s=/index/index/index
index�ļ���index����
```
public  function  index(){
        Session::set('name','hahahahahhp');
        echo 'B��������ȡ����session  user ��' . Session::get('user');
    }
```