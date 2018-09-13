---
layout: post
title: elastic-job中的ShutdownListenerManager
keywords: 
category: elastic-job
author: 晴天
description: 
---

在Elastic-Job提供的运维平台可以直接终止作业，粗略的看了一下。启动项目时，在ListenerManager中初始化各类监听管理器，如ShutdownListenerManager（运行实例关闭监听管理器）。点击终止作业，重写的dataChanged方法中，会通过isRemoveInstance和isReconnectedRegistryCenter判断，终止作业调度。

```
public final class ShutdownListenerManager extends AbstractListenerManager {
    
    private final String jobName;
    
    private final InstanceNode instanceNode;
    
    private final InstanceService instanceService;
    
    private final SchedulerFacade schedulerFacade;
    
    public ShutdownListenerManager(final CoordinatorRegistryCenter regCenter, final String jobName) {
        super(regCenter, jobName);
        this.jobName = jobName;
        instanceNode = new InstanceNode(jobName);
        instanceService = new InstanceService(regCenter, jobName);
        schedulerFacade = new SchedulerFacade(regCenter, jobName);
    }
    
    @Override
    public void start() {
        addDataListener(new InstanceShutdownStatusJobListener());
    }
        class InstanceShutdownStatusJobListener extends AbstractJobListener {
        
        @Override
        protected void dataChanged(final String path, final Type eventType, final String data) {
            if (!JobRegistry.getInstance().isShutdown(jobName) && !JobRegistry.getInstance().getJobScheduleController(jobName).isPaused()
                    && isRemoveInstance(path, eventType) && !isReconnectedRegistryCenter()) {
                schedulerFacade.shutdownInstance();
            }
        }
        
        private boolean isRemoveInstance(final String path, final Type eventType) {
            return instanceNode.isLocalInstancePath(path) && Type.NODE_REMOVED == eventType;
        }
        
        private boolean isReconnectedRegistryCenter() {
            return instanceService.isLocalJobInstanceExisted();
        }
    }
}
```

继承抽象类AbstractListenerManager，实现抽象方法start(开启监听器)

```
public abstract class AbstractListenerManager {
    /**
 	* 作业节点数据访问类.
 	* <p>
 	* 作业节点是在普通的节点前加上作业名称的前缀.
 	* </p> 
 	*/
    private final JobNodeStorage jobNodeStorage;
    
    protected AbstractListenerManager(final CoordinatorRegistryCenter regCenter, final String jobName) {
        jobNodeStorage = new JobNodeStorage(regCenter, jobName);
    }

    /**
     * 开启监听器.
     */
    public abstract void start();
    
    protected void addDataListener(final TreeCacheListener listener) {
        jobNodeStorage.addDataListener(listener);
    }
}
```

其内部类InstanceShutdownStatusJobListener继承抽象类AbstractJobListener（作业注册中心的监听器）

```
public abstract class AbstractJobListener implements TreeCacheListener {
    
    @Override
    public final void childEvent(final CuratorFramework client, final TreeCacheEvent event) throws Exception {
        ChildData childData = event.getData();
        if (null == childData) {
            return;
        }
        String path = childData.getPath();
        if (path.isEmpty()) {
            return;
        }
        dataChanged(path, event.getType(), null == childData.getData() ? "" : new String(childData.getData(), Charsets.UTF_8));
    }
    
    protected abstract void dataChanged(final String path, final Type eventType, final String data);
}
```

这里有一个接口TreeCacheListener（监听变化）

```
public interface TreeCacheListener
{
    /**
     * Called when a change has occurred
     *
     * @param client the client
     * @param event  describes the change
     * @throws Exception errors
     */
    public void childEvent(CuratorFramework client, TreeCacheEvent event) throws Exception;
}
```

