## Runtime

### category 添加成员变量
```Objective-C
- (void)setSafeModeWindow:(UIWindow *)safeModeWindow {
    objc_setAssociatedObject(self, @selector(safeModeWindow), safeModeWindow, OBJC_ASSOCIATION_RETAIN);
}

- (UIWindow *)safeModeWindow {
    return objc_getAssociatedObject(self, @selector(safeModeWindow));
}
```

### 改变原始方法实现（Method swizzled）
```Objective-C
+ (void)load
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [super class];
        
        SEL originalSelector =
        @selector(application:didFinishLaunchingWithOptions:);
        SEL swizzledSelector =
        @selector(bootingProtection_application:didFinishLaunchingWithOptions:);
        
        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
        
        BOOL didAddMethod =
        class_addMethod(class,
                        originalSelector,
                        method_getImplementation(swizzledMethod),
                        method_getTypeEncoding(swizzledMethod));
        
        if (didAddMethod) {
            class_replaceMethod(class,
                                swizzledSelector,
                                method_getImplementation(originalMethod),
                                method_getTypeEncoding(originalMethod));
        }
        else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}

```

### Runtime获取成员变量
- 相关资料：[iOS runtime实战应用：成员变量和属性
](http://www.jianshu.com/p/d361f169423b)

```Objective-C
unsigned int outCount = 0;
    objc_property_t * properties = class_copyPropertyList([Model class], &outCount);
    for (unsigned int i = 0; i < outCount; i ++) {
        objc_property_t property = properties[i];
        //属性名
        const char * name = property_getName(property);
        //属性描述
        const char * propertyAttr = property_getAttributes(property);
        NSLog(@"属性描述为 %s 的 %s ", propertyAttr, name);

        //属性的特性
        unsigned int attrCount = 0;
        objc_property_attribute_t * attrs = property_copyAttributeList(property, &attrCount);
        for (unsigned int j = 0; j < attrCount; j ++) {
            objc_property_attribute_t attr = attrs[j];
            const char * name = attr.name;
            const char * value = attr.value;
            NSLog(@"属性的描述：%s 值：%s", name, value);
        }
        free(attrs);
        NSLog(@"\n");
    }
    free(properties);
```

### 快速归档
```Objective-C
- (id)initWithCoder:(NSCoder *)aDecoder {
    if (self = [super init]) {
        unsigned int outCount;
        Ivar * ivars = class_copyIvarList([self class], &outCount);
        for (int i = 0; i < outCount; i ++) {
            Ivar ivar = ivars[i];
            NSString * key = [NSString stringWithUTF8String:ivar_getName(ivar)];
            [self setValue:[aDecoder decodeObjectForKey:key] forKey:key];
        }
    }
    return self;
}

- (void)encodeWithCoder:(NSCoder *)aCoder {
    unsigned int outCount;
    Ivar * ivars = class_copyIvarList([self class], &outCount);
    for (int i = 0; i < outCount; i ++) {
        Ivar ivar = ivars[i];
        NSString * key = [NSString stringWithUTF8String:ivar_getName(ivar)];
        [aCoder encodeObject:[self valueForKey:key] forKey:key];
    }
}
```