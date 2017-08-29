优先队列

### API
- insert(element)
- max()
- delMax()
- isEmpty()
- size()

### 基于二叉堆（binary heap）的实现 
```js
class MaxPQ {
	constructor () {
		this.heap = [];
		this.n = 0;
	}

	insert (value) {
		this.n +=1;
		this.heap[this.n] = value;
		this._swim[this.n];
	}

	delMax () {
		this.heap[1] = this.heap[this.n];
		this.heap[this.n] = null;
		this.n -= 1;
		this._sink(1);
	}

	_swim (k) {
		while (k > 1) {
			var p = Math.floor(k / 2);
			if (this.heap[k] > this.heap[p]) {
				var temp = this.heap[k];
				this.heap[k] = this.heap[p];
				this.heap[p] = temp;
				k = p;
			}
		}
	}

	_sink (k) {
    while (2k < this.n) {
			var j = 2k;
			if (this.heap[k] >= this.heap[j] && this.heap[k] >= this.heap[j + 1]) {
				return
			}
			if (this.heap[j] < this.heap[j + 1]) {
				j += 1;
			}
			var temp = this.heap[k];
			this.heap[k] = this.heap[j];
			this.heap[j] = this.heap[k];
			k = j;
		}
	}
}
```