# How to Test isAxiosError

I've recently been working on a project where we use typescript and axios to make api calls.

In order to have more clear error pathways, we have subclassed errors with the following methods:
```ts
export class MyError extends Error {}
```

Then we use builders to create errors with the following:
```ts
// note the unknown type from new typescript
export const createMyError = (err: unknown) => {
    if (axios.isAxiosError(err)) {
        // do some things that map response / api errors to a better error message
        return new MyError(err)
    } else {
        // do things that are not api response related to make a better error message
        return new MyError(err)
    }
}
```

I couldn't for the life of me figure out how to test the initial pathway without throwing an error in a promise.

I tried multiple approaches that did not work:
```ts
import {AxiosError} from 'axios'

let testError = new AxiosError('my test error')
expect(createMyError(testerror)).toContain('specific string format from the axios.isAxiosError pathway')
// did not hit the axios.isAxiosError(err) path
```

I also tried this:
```ts
import {AxiosError} from 'axios'

let testError: AxiosError = new Error('my test error') as AxiosError
expect(createMyError(testerror)).toContain('specific string format from the axios.isAxiosError pathway')
// did not hit the axios.isAxiosError(err) path
```

So I was at a loss for a bit.

Then I thought: why don't I check out the axios library itself to see where the isAxiosError function actually exists (thank the heavens for code search).

I went to the github repo, typed in isAxiosError, pulled up (the file)[https://github.com/axios/axios/blob/master/lib/helpers/isAxiosError.js#L10] and immediately facepalmed.

*As long as I passed in an object that had the property .isAxiosError set, I was good* 

Here is how I was able to test that path:
```ts
let testError = {isAxiosError: true}
expect(createMyError(testError)).toContain('what I want it to contain')
```

And we were all good to go.


