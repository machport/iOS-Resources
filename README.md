# iOS-Resources
the code below provides a example on how to grab the lua state on ios roblox, grab the addrs yourself and offsets yourself
```c++
#include <stdio.h>
#include <Foundation/Foundation.h>
#include <mach-o/dyld.h>
#include <arm_neon.h>
@interface InputCapture : UIView @end
namespace Hooks {
    bool game_loaded = false;
    bool hook_applied = false;
    bool init_lua = true;

    long long (*newthread)(long long state);

    long long newthread_hook(long long state) {
        if ((long)__builtin_return_address(0) == newthread_return_addr) {
            while (init_lua) {
                init_lua = false;
                //do ur stuff here
            }
        }
        return newthread(state);
    }
};


%hook InputCapture
-(id)init:(CGRect)arg1 vrMode:(BOOL)arg2 {
    if (!Hooks::game_loaded) {
        Hooks::game_loaded = true;
        return %orig(); 
    } 
    else { 
        if (!Hooks::hook_applied) {
            MSHookFunction((void*)newthread_addr+(long long)_dyld_get_image_vmaddr_slide(0);, (void*)&Hooks::newthread_hook, (void**)&Hooks::newthread);
            Hooks::hook_applied = true;
        }
        Hooks::init_lua = true;
    } 
    return %orig();
}
%end


```
