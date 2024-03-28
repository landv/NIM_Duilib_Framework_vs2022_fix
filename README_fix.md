升级为vs2022项目，sdk 10最新

- 全部编译模式和项目修改属性

  - 平台工具集修改为vs2022 v143
  - windows sdk版本为10最新安装的版本

- async_modal_runner.h文件

  - ```c++
    // TODO FIX landv 修复20240328
    public:
    	template<class _Ty>
    	//friend class std::_Ref_count_obj;
    	friend class std::_Ref_count_obj2;
    
    
    	friend class AsyncModalRunnerManager;
    	friend class std::shared_ptr<AsyncModalRunner>;
    	friend class std::_Ref_count<AsyncModalRunner>;
    
    	AsyncModalRunner(Delegate *delegate);
    	virtual ~AsyncModalRunner();
    
    	void Run();
    private:
    	bool is_running_;
    	bool quit_posted_;
    	Delegate *delegate_;
    	nbase::WaitableEvent event_;
    	std::unique_ptr<ModalWndBase> modal_dlg_;
    ```

- message_loop_proxy.h .cpp

  - 将.h文件里面定义重复的函数放到cpp文件里面

  - ```c++
    // fix landv 20240328 template<>
    void MessageLoopProxy::PostTaskAndReplyRelay<void(), void()>::Run()
    {
    	std_task_();
    	origin_loop_->PostTask(
    		nbase::Bind(&PostTaskAndReplyRelay::RunReplyAndSelfDestruct, this));
    }
    ```



这样就升级成功了





# release模式 错误修复

- 找到错误位置
  - _wsetlocale(LC_ALL, L"chs");
  - 在头文件引用#include "locale"

