
/*  */

/*  */
#include <stdio.h>
#include <string.h>
#include <fcntl.h>      /* open() */
#include <unistd.h>     /* close() */
#include <search.h>     /* tsearch() */
#include <stdlib.h>     /* memcmp() */
/*  */


/*  */

typedef struct eventSet
{
    int type;
    /* 封装后用作lfind/tfind的compar函数 */
    int (*isFocus)(void *dataView, void *dataEvent);
} eventSet_s;

/* 统一 view和control的 data */
struct extra_data
{
    void *data;
} extra_data_s;

typedef struct event
{
    // eventSet_s set;
    int type; /* 这里不需要继承findFocus这个函数 */
    int id;
    extra_data_s data;
} event_s;

typedef struct view
{
    int id;
    extra_data_s data;
    void (*render)(void *data);
} view_s;

typedef struct model
{
    int id;
    void *data;
    void (*func)(event_s eventModel, void *data);
} model_s;


/* 要解决的问题
1. 初始化
    event注册，eventSet要注册（lst），event内不含eventSet，改为type
    model注册（translate eventControl --> eventModel 注册?）
    view注册（bind view2model）
2. translate函数该有哪个class来管理 */

// #include "lstLib.h"

/* void init_event_lst();
void register_event();
void init_model_lst();
void register_model();
void init_view_lst();
void register_view(); */

/* typedef struct _node_event_
{
    NODE node;
    event_s event;
} node_event_s;
typedef struct _node_model_
{
    NODE node;
    model_s model;
} node_model_s;
typedef struct _node_view_
{
    NODE node;
    view_s view;
} node_view_s;


typedef struct _list_with_lock_
{
    LIST list;
    pthread_mutex_t lock;
} list_with_lock_s;

static void init_list(list_with_lock_s *list_with_lock)
{
    memset(list_with_lock, 0, sizeof(list_with_lock_s));
    lstInit(list_with_lock->list);
    pthreadMutexInit(list_with_lock->lock);
}

static list_with_lock_s event_list;
void event_init(int type)
{
    init_list(&event_list);
}

void event_register(int type) */


/* {
    char line[ELSIZE], tab[TABSIZE][ELSIZE];
    size_t nel = 0;
    ...
    void *lsearch(const void *key, void *base, size_t *nelp, size_t width,
                  int (*compar)(const void *, const void *));
    while (fgets(line, ELSIZE, stdin) != NULL && nel < TABSIZE)
        (void) lsearch(line, tab, &nel,
                       ELSIZE, (int (*)(const void *, const void *)) strcmp);
} */

void RegEvent(int type, int compar(extra_data_s, extra_data_s))
{
    static int eventTree = NULL;
    event_s this =
    {
        .type = type,
        .isFocus = compar;
    };

    tsearch(this, eventTree, compareInt);
}

view_s *isFocus(event_s event)
{
    /*
    void *tfind(const void *key, void *const *rootp,
           int(*compar)(const void *, const void *));
            */
    tfind(event->data, viewTree, tfind(event->type, eventTree, compareInt));
}

void RegView(extra_data_s *data, void (*render)(void *data))
{
    static int idView = 0;
    static int viewTree = NULL;
    view_s this =
    {
        .id = atom_add(idView, 1);
        .data = data,
        .render = render;
    };

    tsearch(this, viewTree, compareInt);
}

void RegModel(void *data, void (*func)(event_s eventModel, void *data))
{
    static int idModel = 0;
    static int modelTree = NULL;
    model_s this =
    {
        .id = atom_add(idModel, 1);
        .data = data,
        .func = func;
    };

    tsearch(this, modelTree, compareInt);
}


void user_send_event(event_s *event)
{
    fifoWrite(event);
}

void tsk_viewModel()
{
    event_s *event;
    view_s *view;
    model_s *model;
    while (1)
    {
        fifoRead(event); /* 阻塞 */
        view = isFocus(*event);
        /*
        1. bind_view2model: table
        2. SET??: view(control_event??), model(model_event), event
         */
        model = view2model(view); /* view2model需要先init，两者怎么绑定? */
        model->func(translate1(event->data), model->data); /* event需要注册，model和control是两套event，怎么转换? */
        // view->render(translate2(model->data)); /* 怎么转换? */
        view->render(model->data); /* 这样不用转换 */
    }
}



#if 0


struct event
{
    int id;
    void *data;
};

struct model
{
    id;
    prio;
    *data;
    *func(event, data);
};

struct viewModel
{
    *find(event, modeltree);
};

struct view
{
    *draw(model);
};

/* 针对单一的region */
/* user --> event --> model --viewModel--> view
按键板下键 --> event下 --> model数据更改 --viewModel--> 显示变更 */
/* 多region: (view + user) --> event */

/* dsp交互，透传的拷贝的 */
/* TODO
region交互
event独立（功能/业务无关） */

/* event驱动（只传递event）
control --> viewModel |<-> model
                      |--> view
eg.
    用户按下鼠标，触发event（CLICK, pos），传递给viewModel
    viewModel执行find(event, modeltree);判断是哪个model触发event（tranlate+dispatch）
    viewModel从model获取相关信息model->data（pos, value），然后执行func(event, data);
    viewModel给view获取相关信息model，然后执行draw(model);

    用户按下按键板下键，触发event（DOWN, nil），传递给viewModel
    viewModel执行find(event, modeltree);判断是哪个model触发event（tranlate+dispatch）
    viewModel从model获取相关信息model->data（pos, value），然后执行func(event, data);
    viewModel给view获取相关信息model，然后执行draw(model);
 */

/* eventType来区分不同类型的event
struct eventSet
{
    type;
    *findModel(tree, extraData);
}
struct event
{
    eventSet;
    id;
}
struct message
{
    eventControl;
    *extraData;
}

struct modelSet
{
    type;
    *func(eventControl, eventModel);
};
struct model
{
    modelSet;
    id;
    *data;
    *func(eventModel, data);
};

struct message_translated
{
    modelID;
    eventModel;
    *extraData;
}

struct view
{
    prio;
    *draw(model);
}

*/

#endif


图像缩放

图像的pitch和width都需要
字库的像素数和byte数一定，将其 直接绘制（重点，即直接cpu修改图像内存，pos对应图像内存上对应的点） 在对应的图像上，像素一一对应，但byte数要转换

比如字库1像素byte_per_pixel_pic=1/8个byte，图像1像素byte_per_pixel_font=3个byte，图像width w_pic=3840
那么
row [0, w_font) col [0, h_font)
pPic[row*w_pic*3+col]=_to_pic_type(pFont[row*w_font/8+col])
因为没法一次1/8个byte（最小操作1byte），故一次操作8个pixel（通过bitmask区分pixel），即一次最少操作pixel数满足 pixel（最小整数）= byte_per_operation / byte_per_pixel_font
可得一个数组（8, 4, 8, 2, 8, 4, 8, 1, 8, 4, 8, 2, 8, 4, 8, 1…）（可以看出是公约后的分母），即(8, 4, 2, 8, 4, 8, 1)，对应的byte数就可以得到了
pic同理，然后会遇到两边不一样，这时取公倍数即可；由于pixel数公倍数最大就是8，故图省事直接都取8也可以


带放大，倍率ratio
sub_row, sub_col [0, ratio)
pPic[(row*ratio+sub_row)*w_pic*byte_per_pixel_pic+(col*ratio+sub_col)]=_to_pic_type(pFont[row*w_font*byte_per_pixel_font+col]


图像操作核心：pixel为单位

![](http://upload-images.jianshu.io/upload_images/293860-f0918f2da804ae3a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1080/q/50)


