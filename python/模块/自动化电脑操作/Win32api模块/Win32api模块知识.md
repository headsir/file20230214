

[win32api参考手册]http://www.yfvb.com/help/win32sdk/

pywin32：包含 win32gui、win32api、win32con 3个子模块，用于窗口管理（定位窗口、显示和关闭窗口、获取窗口位置等）

```
mport win32api, win32con, win32gui  # pypiwin32
import time

# 文字对应的键盘值
VK_CODE = {
    'backspace': 0x08,
    'tab': 0x09,
    'clear': 0x0C,
    'enter': 0x0D,
    'shift': 0x10,
    'ctrl': 0x11,
    'alt': 0x12,
    'pause': 0x13,
    'caps_lock': 0x14,
    'esc': 0x18,
    'spacebar': 0x20,
    'page_up': 0x21,
    'page_down': 0x22,
    'end': 0x23,
    'home': 0x24,
    'left_arrow': 0x25,
    'up_arrow': 0x26,
    'right_arrow': 0x27,
    'down_arrow': 0x28,
    'select': 0x29,
    'print': 0x2A,
    'execute': 0x2B,
    'print_screen': 0x2C,
    'ins': 0x2D,
    'del': 0x2E,
    'help': 0x2F,
    '0': 0x30,
    '1': 0x31,
    '2': 0x32,
    '3': 0x33,
    '4': 0x34,
    '5': 0x35,
    '6': 0x36,
    '7': 0x37,
    '8': 0x38,
    '9': 0x39,
    'a': 0x41,
    'b': 0x42,
    'c': 0x43,
    'd': 0x44,
    'e': 0x45,
    'f': 0x46,
    'g': 0x47,
    'h': 0x48,
    'i': 0x49,
    'j': 0x4A,
    'k': 0x4B,
    'l': 0x4C,
    'm': 0x4D,
    'n': 0x4E,
    'o': 0x4F,
    'p': 0x50,
    'q': 0x51,
    'r': 0x52,
    's': 0x53,
    't': 0x54,
    'u': 0x55,
    'v': 0x56,
    'w': 0x57,
    'x': 0x58,
    'y': 0x59,
    'z': 0x5A,
    'numpad_0': 0x60,  # 数字键盘
    'numpad_1': 0x61,
    'numpad_2': 0x62,
    'numpad_3': 0x63,
    'numpad_4': 0x64,
    'numpad_5': 0x65,
    'numpad_6': 0x66,
    'numpad_7': 0x67,
    'numpad_8': 0x68,
    'numpad_9': 0x69,
    'multiply_key': 0x6A,
    'add_key': 0x6B,
    'separator_key': 0x6C,
    'subtract_key': 0x6D,
    'decimal_key': 0x6E,
    'divide_key': 0x6F,
    'F1': 0x70,
    'F2': 0x71,
    'F3': 0x72,
    'F4': 0x73,
    'F5': 0x74,
    'F6': 0x75,
    'F7': 0x76,
    'F8': 0x77,
    'F9': 0x78,
    'F10': 0x79,
    'F11': 0x7A,
    'F12': 0x7B,
    'F13': 0x7C,
    'F14': 0x7D,
    'F15': 0x7E,
    'F16': 0x7F,
    'F17': 0x80,
    'F18': 0x81,
    'F19': 0x82,
    'F20': 0x83,
    'F21': 0x84,
    'F22': 0x85,
    'F23': 0x86,
    'F24': 0x87,
    'num_lock': 0x90,
    'scroll_lock': 0x91,
    'left_shift': 0xA0,
    'right_shift': 0xA1,
    'left_control': 0xA2,
    'right_control': 0xA3,
    'left_menu': 0xA4,
    'right_menu': 0xA5,
    'browser_back': 0xA6,
    'browser_forward': 0xA7,
    'browser_refresh': 0xA8,
    'browser_stop': 0xA9,
    'browser_search': 0xAA,
    'browser_favorites': 0xAB,
    'browser_start_and_home': 0xAC,
    'volume_mute': 0xAD,
    'volume_Down': 0xAE,
    'volume_up': 0xAF,
    'next_track': 0xB0,
    'previous_track': 0xB1,
    'stop_media': 0xB2,
    'play/pause_media': 0xB3,
    'start_media': 0xB4,
    'select_media': 0xB5,
    'start_application_1': 0xB6,
    'start_application_2': 0xB7,
    'attn_key': 0xF6,
    'crsel_key': 0xF7,
    'exsel_key': 0xF8,
    'play_key': 0xFA,
    'zoom_key': 0xFB,
    'clear_key': 0xFE,
    ';': 0xBA,
    '+': 0xBB,
    ',': 0xBC,
    '-': 0xBD,
    '.': 0xBE,
    '/': 0xBF,
    '~': 0xC0,
    '[': 0xDB,
    '\\': 0xDC,
    ']': 0xDD,
    "'": 0xDE,
    '`': 0xC0
}

# 获取窗口
def get_window(name):
    handle = win32gui.FindWindow(0, name)
    # 获取窗口句柄
    if handle == 0:
        return None
    else:
        # 返回handle
        return handle


# 输入文字VK_CODE[word]为要输入的文字码
def key_input(input_word=''):
    for word in input_word:
        win32api.keybd_event(VK_CODE[word], 0, 0, 0)  # 按下键
        win32api.keybd_event(VK_CODE[word], 0, win32con.KEYEVENTF_KEYUP, 0)  # 松开按键
        time.sleep(0.1)
    time.sleep(2)


# 根据坐标获取窗口，窗口必须在最上层
def get_win_handle(pos):
    return win32gui.WindowFromPoint(pos)


# 获取坐标值
def get_curpos():
    return win32gui.GetCursorPos()


def mouse_left_click(new_x, new_y):
    win32api.SetCursorPos(new_x, new_y)  # 设置坐标
    time.sleep(1)
    win32api.mouse_event(win32con.MOUSEEVENTF_LEFTDOWN, 0, 0, 0, 0)  # 鼠标按下
    time.sleep(0.1)
    win32api.mouse_event(win32con.MOUSEEVENTF_LEFTUP, 0, 0, 0, 0)  # 鼠标抬起


def shotImg(pos):
    pass


# handle = get_window("文件传输助手")
handle = get_window("新建 文本文档.txt - 记事本")

# handle = get_win_handle((952,265))

print(handle)
if handle is None:
    print("未获取到窗口")
else:
    # 发送还原最小化窗口的信息
    win32gui.SendMessage(handle, win32con.WM_SYSCOMMAND, win32con.SC_RESTORE, 0)
    (x1, y1, x2, y2) = win32gui.GetWindowRect(handle)
    # 打印坐标
    print(x1, y1, x2, y2)
    # 窗口放到最上层
    win32gui.SetForegroundWindow(handle)

    time.sleep(1)
    key_input('yyyy')
    # win32gui.SendMessage(handle, win32con.WM_SETTEXT, None, 'hello')  # 给窗口发消息
    win32gui.PostMessage(handle, win32con.WM_KEYDOWN, win32con.VK_RETURN, 0)
    win32gui.PostMessage(handle, win32con.WM_KEYUP, win32con.VK_RETURN, 0)
```

