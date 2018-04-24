---
layout: post
title: chain demo
keywords: 
category: chain
author: 晴天
description: 
---

```
        <dependency>
            <groupId>commons-chain</groupId>
            <artifactId>commons-chain</artifactId>
            <version>1.2</version>
        </dependency>
```

```
public class Command1 implements Command{
    public boolean execute(Context context) throws Exception {
        System.out.println("Command1 is done");
        return false;
    }
}
```

```
public class Command2 implements Command{
    public boolean execute(Context context) throws Exception {
        System.out.println("Command2 is done");
        return false;
    }
}
```

```
public class Command3 implements Command{
    public boolean execute(Context context) throws Exception {
        System.out.println("Command3 is done");
        return false;
    }
}
```

```
public class Filter1 implements Filter{
    public boolean postprocess(Context context, Exception e) {
        System.out.println("Filter1 is after done");
        return false;
    }

    public boolean execute(Context context) throws Exception {
        System.out.println("Filter1 is done");
        return false;
    }
}
```

```
public class Filter2 implements Filter{
    public boolean postprocess(Context context, Exception e) {
        System.out.println("Filter2 is after done");
        return false;
    }

    public boolean execute(Context context) throws Exception {
        System.out.println("Filter2 is done");
        return false;
    }
}
```

```
public class CommandChain extends ChainBase{
    public CommandChain() {
        addCommand(new Filter2());
        addCommand(new Filter1());
        addCommand(new Command1());
        addCommand(new Command2());
        addCommand(new Command3());
    }

    public static void main(String[] args) throws Exception {
        Command process = new CommandChain();
        Context ctx = new ContextBase();
        process.execute(ctx);
    }
}
```

<p>执行结果：</p>

```
Filter2 is done
Filter1 is done
Command1 is done
Command2 is done
Command3 is done
Filter1 is after done
Filter2 is after done
```

<p>execute方法源码：</p>

```
/**
     * See the {@link Chain} JavaDoc.
     *
     * @param context The {@link Context} to be processed by this
     *  {@link Chain}
     *
     * @throws Exception if thrown by one of the {@link Command}s
     *  in this {@link Chain} but not handled by a <code>postprocess()</code>
     *  method of a {@link Filter}
     * @throws IllegalArgumentException if <code>context</code>
     *  is <code>null</code>
     *
     * @return <code>true</code> if the processing of this {@link Context}
     *  has been completed, or <code>false</code> if the processing
     *  of this {@link Context} should be delegated to a subsequent
     *  {@link Command} in an enclosing {@link Chain}
     */
    public boolean execute(Context context) throws Exception {

        // Verify our parameters
        if (context == null) {
            throw new IllegalArgumentException();
        }

        // Freeze the configuration of the command list
        frozen = true;

        // Execute the commands in this list until one returns true
        // or throws an exception
        boolean saveResult = false;
        Exception saveException = null;
        int i = 0;
        int n = commands.length;
        for (i = 0; i < n; i++) {
            try {
                saveResult = commands[i].execute(context);
                if (saveResult) {
                    break;
                }
            } catch (Exception e) {
                saveException = e;
                break;
            }
        }

        // Call postprocess methods on Filters in reverse order
        if (i >= n) { // Fell off the end of the chain
            i--;
        }
        boolean handled = false;
        boolean result = false;
        //因为是--，所以执行结果最后的filter顺序是反的
        for (int j = i; j >= 0; j--) {
            if (commands[j] instanceof Filter) {
                try {
                    result =
                        ((Filter) commands[j]).postprocess(context,
                                                           saveException);
                    if (result) {
                        handled = true;
                    }
                } catch (Exception e) {
                      // Silently ignore
                }
            }
        }

        // Return the exception or result state from the last execute()
        if ((saveException != null) && !handled) {
            throw saveException;
        } else {
            return (saveResult);
        }

    }
```

